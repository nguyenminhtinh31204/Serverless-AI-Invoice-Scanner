---
title: "Create Lambda Function #1"
weight: 2
chapter: false
pre: " <b> 3.2 </b> "
---

#### Overview

In this step, you will create the first Lambda Function named **UploadInvoiceFileFunction**. This function is triggered when a user uploads an invoice file to the S3 bucket. It then uses **Amazon Textract** to extract text, **OpenAI API** to analyze and understand the content, and finally stores the extracted information into **DynamoDB**.

---

#### Step 1: Open the Lambda Console

1. Log in to the [AWS Console](https://console.aws.amazon.com/), search for **Lambda**, and select **Lambda**.

![Open Lambda](/images/3.lambdafunctions/3.2-uploadinvoicelambda/001-openlambda.png)

{{% notice warning %}}
⚠️ **Note**: Make sure you are in the correct **Region: Singapore (ap-southeast-1)** before creating the Lambda function. This is the region where you created the S3 bucket, DynamoDB table. If you choose the wrong region, Lambda won't be able to access the system's other services.
{{% /notice %}}

2. Click **Create function**.

![Create Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/002-createfunction.png)

#### Step 2: Configure the Lambda Function

1. Under **Author from scratch**, fill in the following details:

    - **Function name**: `UploadInvoiceFileFunction`
    - **Runtime**: `Python 3.12`
    - **Architecture**: `x86_64`
    - **Permissions**: Choose **Use an existing role**
    - **Existing role**: `LambdaExecutionRole-AIInvoiceScanner` (created in the previous step)

![Configure Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/003-configurefunction.png)

2. Click **Create function** to proceed.

![Configure Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/004-createfunction.png)

---

#### Step 3: Add Python Code

1. Once the function is created, scroll down to the **Code** section in the Lambda interface.

2. Paste the **entire Python code** below, replacing the default content:

```python
import boto3
import json
import uuid
import base64
import re
import urllib.parse
import urllib.request
import urllib.error
import os
import time
import datetime
import hashlib
from decimal import Decimal

# ============================================================
# AWS CONFIG
# ============================================================
s3 = boto3.client("s3")
textract = boto3.client("textract")
dynamodb = boto3.resource("dynamodb")

BUCKET_NAME = os.environ.get("BUCKET_NAME")
if not BUCKET_NAME:
    raise ValueError("Missing BUCKET_NAME environment variable")

DYNAMO_TABLE_NAME = os.environ.get("DYNAMO_TABLE_NAME", "InvoiceData")
table = dynamodb.Table(DYNAMO_TABLE_NAME)

# ============================================================
# APP CONFIG
# ============================================================
UPLOAD_PREFIX = os.environ.get("UPLOAD_PREFIX", "uploads/")
ALLOWED_EXTENSIONS = (".png", ".jpg", ".jpeg", ".pdf")

# Chỉ dùng 1 model OpenAI.
# Khuyên dùng: gpt-4.1-mini cho ổn định. Nếu tài khoản còn hỗ trợ nano, có thể đặt OPENAI_MODEL=gpt-4.1-nano.
OPENAI_MODEL = os.environ.get("OPENAI_MODEL", "gpt-4.1-mini")

# Giới hạn output để tiết kiệm token
MAX_OUTPUT_TOKENS = int(os.environ.get("MAX_OUTPUT_TOKENS", "250"))

# Giới hạn text OCR gửi lên AI để tránh tốn token khi hóa đơn quá dài
MAX_OCR_CHARS = int(os.environ.get("MAX_OCR_CHARS", "6000"))


# ============================================================
# RESPONSE HELPERS
# ============================================================
def decimal_default(obj):
    if isinstance(obj, Decimal):
        return float(round(obj, 2))
    raise TypeError("Type not serializable")


def make_response(status_code, body_dict):
    return {
        "statusCode": status_code,
        "headers": {
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Credentials": "true",
            "Access-Control-Allow-Headers": "*",
            "Access-Control-Allow-Methods": "OPTIONS,GET,POST"
        },
        "body": json.dumps(body_dict, default=decimal_default, ensure_ascii=False)
    }


# ============================================================
# BASIC HELPERS
# ============================================================
def clean_filename(filename):
    filename = filename or f"invoice_{uuid.uuid4().hex}.png"
    filename = filename.split("/")[-1].split("\\")[-1]
    return re.sub(r"[^a-zA-Z0-9_\-\.]", "_", filename)


def get_content_type(filename):
    lower = filename.lower()
    if lower.endswith(".png"):
        return "image/png"
    if lower.endswith((".jpg", ".jpeg")):
        return "image/jpeg"
    if lower.endswith(".pdf"):
        return "application/pdf"
    return "application/octet-stream"


def is_allowed_file(key):
    return key.lower().endswith(ALLOWED_EXTENSIONS)


def compact_ocr_text(text):
    """
    Tối ưu token:
    - Xóa khoảng trắng dư
    - Bỏ dòng trống
    - Nếu quá dài, giữ phần đầu và phần cuối vì tổng tiền thường nằm cuối hóa đơn
    """
    lines = []
    for line in text.splitlines():
        line = re.sub(r"\s+", " ", line).strip()
        if line:
            lines.append(line)

    compacted = "\n".join(lines)

    if len(compacted) <= MAX_OCR_CHARS:
        return compacted

    head_size = int(MAX_OCR_CHARS * 0.65)
    tail_size = MAX_OCR_CHARS - head_size

    return (
        compacted[:head_size]
        + "\n...\n[OCR_TEXT_TRUNCATED_TO_SAVE_TOKENS]\n...\n"
        + compacted[-tail_size:]
    )


def safe_parse_json(ai_text):
    try:
        text = ai_text.strip()

        # Nếu model lỡ bọc ```json ... ```
        text = re.sub(r"^```json\s*", "", text, flags=re.IGNORECASE)
        text = re.sub(r"^```\s*", "", text)
        text = re.sub(r"\s*```$", "", text)

        match = re.search(r"(\{.*\})", text, re.DOTALL)
        json_str = match.group(1) if match else text

        return json.loads(json_str, parse_float=Decimal)
    except Exception as e:
        print("❌ Parse JSON error:", str(e))
        print("Raw AI text preview:", ai_text[:300])
        return None


def normalize_invoice_data(data):
    """
    Đảm bảo DynamoDB luôn có schema ổn định.
    """
    if not isinstance(data, dict):
        data = {}

    normalized = {
        "InvoiceNumber": data.get("InvoiceNumber"),
        "CustomerName": data.get("CustomerName"),
        "InvoiceDate": data.get("InvoiceDate"),
        "TotalAmount": data.get("TotalAmount")
    }

    # Chuẩn hóa chuỗi rỗng thành null
    for key in ["InvoiceNumber", "CustomerName", "InvoiceDate"]:
        value = normalized.get(key)
        if isinstance(value, str):
            value = value.strip()
            normalized[key] = value if value else None

    # Chuẩn hóa TotalAmount
    amount = normalized.get("TotalAmount")
    if amount in ["", None]:
        normalized["TotalAmount"] = None
    else:
        try:
            normalized["TotalAmount"] = Decimal(str(amount))
        except Exception:
            normalized["TotalAmount"] = None

    return normalized


def create_file_id(bucket, key):
    """
    Tạo ID cố định theo S3 object metadata.
    Giúp upload/click lại cùng một file thì không gọi Textract/OpenAI lại.
    """
    try:
        head = s3.head_object(Bucket=bucket, Key=key)
        etag = head.get("ETag", "").replace('"', "")
        size = str(head.get("ContentLength", ""))
        last_modified = str(head.get("LastModified", ""))
        raw = f"{bucket}|{key}|{etag}|{size}|{last_modified}"
    except Exception as e:
        print("⚠️ Cannot read S3 metadata for hash, fallback to bucket/key:", str(e))
        raw = f"{bucket}|{key}"

    digest = hashlib.sha256(raw.encode("utf-8")).hexdigest()
    return f"FILE_{digest[:24]}", digest


def get_existing_success(invoice_id):
    """
    Cache theo InvoiceId.
    Nếu file đã xử lý SUCCESS rồi thì không gọi Textract/OpenAI nữa.
    """
    try:
        response = table.get_item(Key={"InvoiceId": invoice_id})
        item = response.get("Item")
        if item and item.get("ProcessStatus") == "SUCCESS":
            return item
    except Exception as e:
        print("⚠️ DynamoDB get_item failed:", str(e))
    return None


# ============================================================
# TEXTRACT
# ============================================================
def extract_text_with_textract(bucket, key):
    """
    Đọc text bằng Amazon Textract.
    Với hóa đơn ảnh PNG/JPG, detect_document_text đủ cho lab.
    Nếu muốn chuyên hóa đơn hơn, có thể đổi sang analyze_expense.
    """
    response = textract.detect_document_text(
        Document={
            "S3Object": {
                "Bucket": bucket,
                "Name": key
            }
        }
    )

    lines = [
        block.get("Text", "")
        for block in response.get("Blocks", [])
        if block.get("BlockType") == "LINE" and block.get("Text")
    ]

    raw_text = "\n".join(lines)
    return compact_ocr_text(raw_text)


# ============================================================
# OPENAI ONLY
# ============================================================
def build_prompt(extracted_text):
    """
    Prompt ngắn để tiết kiệm token.
    response_format=json_object sẽ ép model trả JSON hợp lệ.
    """
    return (
        "Extract invoice data from OCR text. Return only JSON with exactly these keys:\n"
        "{"
        "\"InvoiceNumber\": null, "
        "\"CustomerName\": null, "
        "\"InvoiceDate\": null, "
        "\"TotalAmount\": null"
        "}\n"
        "Rules: InvoiceDate=YYYY-MM-DD if possible. TotalAmount=number only. "
        "Use null if missing. No explanation.\n\n"
        "OCR_TEXT:\n"
        f"{extracted_text}"
    )


def call_openai(extracted_text):
    api_key = os.environ.get("OPENAI_API_KEY")
    if not api_key:
        raise ValueError("Missing OPENAI_API_KEY environment variable")

    url = "https://api.openai.com/v1/chat/completions"

    payload = {
        "model": OPENAI_MODEL,
        "messages": [
            {
                "role": "system",
                "content": "You are an invoice parser. You must output valid JSON only."
            },
            {
                "role": "user",
                "content": build_prompt(extracted_text)
            }
        ],
        "temperature": 0,
        "max_tokens": MAX_OUTPUT_TOKENS,
        "response_format": {
            "type": "json_object"
        }
    }

    data = json.dumps(payload).encode("utf-8")

    request = urllib.request.Request(
        url,
        data=data,
        method="POST",
        headers={
            "Content-Type": "application/json",
            "Authorization": f"Bearer {api_key}"
        }
    )

    try:
        with urllib.request.urlopen(request, timeout=25) as response:
            result = json.loads(response.read().decode("utf-8"))
            ai_text = result["choices"][0]["message"]["content"]

            usage = result.get("usage", {})
            print(
                "✅ OpenAI success | "
                f"model={OPENAI_MODEL} | "
                f"prompt_tokens={usage.get('prompt_tokens')} | "
                f"completion_tokens={usage.get('completion_tokens')} | "
                f"total_tokens={usage.get('total_tokens')}"
            )

            return ai_text, usage

    except urllib.error.HTTPError as e:
        error_body = e.read().decode("utf-8")
        error_msg = f"OpenAI API error: {e.code} - {error_body}"
        print("❌", error_msg)
        raise Exception(error_msg)

    except Exception as e:
        print("❌ OpenAI connection error:", str(e))
        raise


def call_openai_with_retry(extracted_text, max_retries=3):
    last_exception = None

    for attempt in range(1, max_retries + 1):
        try:
            return call_openai(extracted_text)

        except Exception as e:
            error_msg = str(e)
            last_exception = e
            print(f"❌ OpenAI error attempt {attempt}: {error_msg}")

            # Lỗi tiền/key/quota thì không retry để tránh tốn thời gian
            if any(word in error_msg.lower() for word in ["quota", "insufficient", "invalid api key", "unauthorized", "401"]):
                raise

            # Retry cho rate limit hoặc lỗi server
            if "429" in error_msg or "rate" in error_msg.lower() or any(code in error_msg for code in ["500", "502", "503", "504"]):
                wait_seconds = min((2 ** attempt) * 2, 20)
                print(f"⏳ Retrying OpenAI in {wait_seconds}s...")
                time.sleep(wait_seconds)
                continue

            raise

    raise last_exception


# ============================================================
# DYNAMODB
# ============================================================
def save_invoice_item(
    invoice_id,
    bucket,
    key,
    file_hash,
    status,
    extracted_data,
    openai_usage=None,
    error_message=None,
    ocr_preview=None
):
    item = {
        "InvoiceId": invoice_id,
        "S3Path": f"s3://{bucket}/{key}",
        "Bucket": bucket,
        "S3Key": key,
        "FileHash": file_hash,
        "ProcessStatus": status,
        "ExtractedData": extracted_data,
        "Tags": [],
        "ProcessedAt": datetime.datetime.utcnow().isoformat()
    }

    if openai_usage:
        item["OpenAIUsage"] = {
            "PromptTokens": openai_usage.get("prompt_tokens"),
            "CompletionTokens": openai_usage.get("completion_tokens"),
            "TotalTokens": openai_usage.get("total_tokens")
        }

    if error_message:
        item["ErrorMessage"] = str(error_message)[:1000]

    if ocr_preview:
        item["OcrPreview"] = ocr_preview[:500]

    table.put_item(Item=item)
    return item


# ============================================================
# PROCESS S3 OBJECT
# ============================================================
def process_s3_object(bucket, key):
    print(f"📥 Processing S3 object: s3://{bucket}/{key}")

    if not key.startswith(UPLOAD_PREFIX):
        print(f"⏭️ Skip object outside upload prefix: {key}")
        return {
            "key": key,
            "status": "SKIPPED",
            "reason": f"Object is not under prefix {UPLOAD_PREFIX}"
        }

    if not is_allowed_file(key):
        print(f"⏭️ Unsupported file type: {key}")
        return {
            "key": key,
            "status": "SKIPPED",
            "reason": "Only PNG, JPG, JPEG or PDF are supported"
        }

    invoice_id, file_hash = create_file_id(bucket, key)

    # Cache: nếu file đã xử lý thành công rồi thì không tốn Textract/OpenAI lần nữa
    existing = get_existing_success(invoice_id)
    if existing:
        print(f"♻️ Cache hit. Skip Textract/OpenAI for InvoiceId={invoice_id}")
        return {
            "key": key,
            "invoice_id": invoice_id,
            "status": "CACHED",
            "data": existing
        }

    # 1. Textract OCR
    try:
        extracted_text = extract_text_with_textract(bucket, key)
    except Exception as e:
        error_msg = f"Textract failed: {str(e)}"
        print("❌", error_msg)

        item = save_invoice_item(
            invoice_id=invoice_id,
            bucket=bucket,
            key=key,
            file_hash=file_hash,
            status="FAILED",
            extracted_data={"error": error_msg},
            error_message=error_msg
        )

        return {
            "key": key,
            "invoice_id": invoice_id,
            "status": "FAILED",
            "error": error_msg,
            "data": item
        }

    if len(extracted_text.strip()) < 20:
        error_msg = "No valid text found in the document."
        print("❌", error_msg)

        item = save_invoice_item(
            invoice_id=invoice_id,
            bucket=bucket,
            key=key,
            file_hash=file_hash,
            status="FAILED",
            extracted_data={"error": error_msg},
            error_message=error_msg,
            ocr_preview=extracted_text
        )

        return {
            "key": key,
            "invoice_id": invoice_id,
            "status": "FAILED",
            "error": error_msg,
            "data": item
        }

    # 2. OpenAI chuẩn hóa JSON
    try:
        ai_text, usage = call_openai_with_retry(extracted_text)
        parsed_data = safe_parse_json(ai_text)

        if parsed_data is None:
            status = "FAILED"
            normalized_data = {
                "error": "Invalid JSON from OpenAI",
                "raw_ai_text": ai_text[:500]
            }
        else:
            status = "SUCCESS"
            normalized_data = normalize_invoice_data(parsed_data)

    except Exception as e:
        error_msg = str(e)
        print("❌ OpenAI processing failed:", error_msg)

        item = save_invoice_item(
            invoice_id=invoice_id,
            bucket=bucket,
            key=key,
            file_hash=file_hash,
            status="FAILED",
            extracted_data={"error": error_msg},
            error_message=error_msg,
            ocr_preview=extracted_text
        )

        return {
            "key": key,
            "invoice_id": invoice_id,
            "status": "FAILED",
            "error": error_msg,
            "data": item
        }

    # 3. Lưu DynamoDB
    item = save_invoice_item(
        invoice_id=invoice_id,
        bucket=bucket,
        key=key,
        file_hash=file_hash,
        status=status,
        extracted_data=normalized_data,
        openai_usage=usage,
        error_message=None if status == "SUCCESS" else "Invalid JSON from OpenAI",
        ocr_preview=extracted_text
    )

    print(f"✅ DynamoDB saved | InvoiceId={invoice_id} | status={status}")

    return {
        "key": key,
        "invoice_id": invoice_id,
        "status": status,
        "data": item
    }


# ============================================================
# HANDLER: API GATEWAY UPLOAD + S3 TRIGGER
# ============================================================
def lambda_handler(event, context):
    # Không print toàn bộ event vì API Gateway có thể chứa ảnh base64 rất dài
    print("🔍 Incoming event keys:", list(event.keys()))

    try:
        # Preflight CORS
        if event.get("httpMethod") == "OPTIONS":
            return make_response(200, {"message": "OK"})

        # ------------------------------------------------------------
        # CASE 1: API Gateway Upload
        # Body dạng JSON:
        # {
        #   "file": "data:image/png;base64,...",
        #   "filename": "demo_invoice.png"
        # }
        # ------------------------------------------------------------
        if "body" in event and isinstance(event.get("body"), str) and "Records" not in event:
            print("🌐 API Gateway upload request received")

            body_text = event["body"]

            # Nếu API Gateway encode toàn bộ body base64
            if event.get("isBase64Encoded"):
                body_text = base64.b64decode(body_text).decode("utf-8")

            body = json.loads(body_text)

            if "file" not in body:
                return make_response(400, {"error": "Missing file field"})

            b64_string = body["file"]
            if "," in b64_string:
                b64_string = b64_string.split(",", 1)[1]

            try:
                file_data = base64.b64decode(b64_string)
            except Exception:
                return make_response(400, {"error": "Invalid base64 file"})

            original_filename = clean_filename(body.get("filename", f"invoice_{uuid.uuid4().hex}.png"))

            if not is_allowed_file(original_filename):
                return make_response(400, {"error": "Only PNG, JPG, JPEG or PDF are supported"})

            # Thêm uuid để tránh ghi đè file cũ
            key = f"{UPLOAD_PREFIX}{uuid.uuid4().hex}_{original_filename}"

            s3.put_object(
                Bucket=BUCKET_NAME,
                Key=key,
                Body=file_data,
                ContentType=get_content_type(original_filename)
            )

            print(f"✅ Uploaded to s3://{BUCKET_NAME}/{key}")

            # Không xử lý AI ngay trong upload request.
            # S3 trigger sẽ gọi Lambda lần 2 để xử lý OCR + OpenAI.
            return make_response(200, {
                "message": "Upload successful. Processing will start from S3 trigger.",
                "bucket": BUCKET_NAME,
                "key": key,
                "s3_path": f"s3://{BUCKET_NAME}/{key}"
            })

        # ------------------------------------------------------------
        # CASE 2: S3 Trigger
        # ------------------------------------------------------------
        if "Records" in event and event["Records"] and "s3" in event["Records"][0]:
            print(f"📦 S3 trigger records: {len(event['Records'])}")

            results = []

            for record in event["Records"]:
                event_name = record.get("eventName", "")

                if not event_name.startswith("ObjectCreated:"):
                    print(f"⏭️ Skip non-created event: {event_name}")
                    continue

                bucket = record["s3"]["bucket"]["name"]
                raw_key = record["s3"]["object"]["key"]
                key = urllib.parse.unquote_plus(raw_key)

                result = process_s3_object(bucket, key)
                results.append(result)

            return make_response(200, {
                "message": "S3 event processed",
                "results": results
            })

        return make_response(400, {"error": "Invalid request format"})

    except Exception as e:
        print("❌ Fatal error:", str(e))
        return make_response(500, {
            "error": "Internal Server Error",
            "details": str(e)
        })

```

![Paste Code](/images/3.lambdafunctions/3.2-uploadinvoicelambda/005-sourcecode.png)

3. Click **Deploy** to apply the changes.

![Deploy](/images/3.lambdafunctions/3.2-uploadinvoicelambda/006-deploy.png)

---

#### Step 4: Configure Timeout and Memory

1. Go to the **Configuration > General configuration** tab.
2. Click **Edit**.

![Deploy](/images/3.lambdafunctions/3.2-uploadinvoicelambda/007-configuration.png)

3. Change the following values:

    - **Memory (MB)**: `1024`
    - **Timeout**: `1 minute`

![Edit Memory Timeout](/images/3.lambdafunctions/3.2-uploadinvoicelambda/008-configtimeout.png)

4. **Configuration -> Environment variables**, click **Edit**,

![Set Environment variables](/images/3.lambdafunctions/3.4-fetchinvoicelambda/010-more.png)

5. Click **Save**.

![Click Save](/images/3.lambdafunctions/3.2-uploadinvoicelambda/009-clicksave.png)

---

#### Conclusion

You have successfully created the first Lambda function in the system: UploadInvoiceFileFunction.

 - This function receives invoice files from the React frontend through API Gateway.
 - It decodes the Base64 file content and uploads the file to the uploads/ folder in Amazon S3.
 - After the file is uploaded to S3, the next processing Lambda can be triggered to extract invoice data using Amazon Textract, normalize the data with the OpenAI API, and store the result in DynamoDB.
