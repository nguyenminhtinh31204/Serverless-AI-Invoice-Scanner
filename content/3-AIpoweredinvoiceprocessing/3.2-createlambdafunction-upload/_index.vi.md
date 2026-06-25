---
title: "Tạo Lambda Function #1"
weight: 2
chapter: false
pre: " <b> 3.2 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo Lambda Function đầu tiên có tên là **UploadInvoiceFileFunction**. Function này sẽ được kích hoạt khi người dùng tải lên một file hóa đơn vào S3 bucket. Sau đó, nó sẽ sử dụng **Amazon Textract** để trích xuất văn bản, **Amazon Bedrock** để phân tích và hiểu nội dung, cuối cùng lưu thông tin vào **DynamoDB**.

---

#### Bước 1: Truy cập Lambda Console

1. Đăng nhập vào [AWS Console](https://console.aws.amazon.com/), tìm **Lambda**, sau đó chọn **Lambda**.

![Open Lambda](/images/3.lambdafunctions/3.2-uploadinvoicelambda/001-openlambda.png)

{{% notice warning %}}
⚠️ **Lưu ý**: Hãy đảm bảo bạn đang ở đúng **Region: N. Virginia (us-east-1)** trước khi tạo Lambda function. Đây là vùng bạn đã tạo S3 bucket, DynamoDB table và đăng ký Amazon Bedrock. Nếu chọn sai vùng, Lambda sẽ không thể truy cập được các dịch vụ khác của hệ thống.
{{% /notice %}}

2. Nhấn **Create function**.

![Create Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/002-createfunction.png)

#### Bước 2: Cấu hình Lambda Function

1. Ở mục **Author from scratch**, nhập các thông tin sau:

    - **Function name**: `UploadInvoiceFileFunction`
    - **Runtime**: `Python 3.12`
    - **Architecture**: `x86_64`
    - **Permissions**: Chọn **Use an existing role**
    - **Existing role**: `LambdaExecutionRole-AIInvoiceScanner` (đã tạo ở bước trước)

![Configure Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/003-configurefunction.png)

2. Nhấn **Create function** để khởi tạo.

![Configure Function](/images/3.lambdafunctions/3.2-uploadinvoicelambda/004-createfunction.png)

---

#### Bước 3: Thêm mã nguồn Python

1. Sau khi function được tạo, cuộn xuống phần **Code** trong giao diện Lambda.

2. Dán **toàn bộ đoạn mã Python** dưới đây vào, thay thế nội dung mặc định:

```python
import boto3
import json
import uuid
import base64
import re
import time
from decimal import Decimal

# AWS clients
s3 = boto3.client('s3')
textract = boto3.client('textract')
dynamodb = boto3.resource('dynamodb')
bedrock = boto3.client('bedrock-runtime', region_name='us-east-1')

# Constants
BUCKET_NAME = 'invoice-upload-s3-bucket'
table = dynamodb.Table('InvoiceData')

def decimal_default(obj):
    if isinstance(obj, Decimal):
        return float(round(obj, 2))

def make_response(status_code, body_dict):
    return {
        'statusCode': status_code,
        'headers': {
            'Access-Control-Allow-Origin': '*',
            'Access-Control-Allow-Credentials': True,
            'Access-Control-Allow-Headers': '*',
            'Access-Control-Allow-Methods': 'OPTIONS,POST'
        },
        'body': json.dumps(body_dict, default=decimal_default)
    }

def lambda_handler(event, context):
    print("🔍 Incoming event:")
    print(json.dumps(event))

    try:
        # === CASE 1: API Gateway POST upload file ===
        if "body" in event and isinstance(event['body'], str) and not ("Records" in event):
            body = json.loads(event['body'])
            file_data = base64.b64decode(body['file'])
            filename = body.get('filename', f'invoice_{uuid.uuid4()}.png')
            key = f'uploads/{filename}'

            s3.put_object(
                Bucket=BUCKET_NAME,
                Key=key,
                Body=file_data,
                ContentType='image/png'
            )

            print(f"✅ Uploaded '{key}'")
            return make_response(200, {
                'message': 'Upload thành công',
                's3_path': f's3://{BUCKET_NAME}/{key}'
            })

        # === CASE 2: S3 Trigger xử lý hóa đơn ===
        elif "Records" in event and "s3" in event["Records"][0]:
            bucket = event['Records'][0]['s3']['bucket']['name']
            key = event['Records'][0]['s3']['object']['key']
            print(f"📥 Trigger từ S3: bucket={bucket}, key={key}")

            # 1. Textract
            textract_response = textract.detect_document_text(
                Document={'S3Object': {'Bucket': bucket, 'Name': key}}
            )
            extracted_text = ' '.join(
                [block['Text'] for block in textract_response['Blocks'] if block['BlockType'] == 'LINE']
            )

            if len(extracted_text.strip()) < 20:
                return make_response(400, {'error': 'Không phát hiện được nội dung hợp lệ từ ảnh.'})

            # 2. Prompt cho Bedrock
            prompt = (
                "You are a precise invoice data parser. Extract structured data from the invoice text below.\n"
                "Return ONLY valid JSON with fields:\n"
                "{\n"
                "  \"InvoiceId\": \"\",\n"
                "  \"CustomerName\": \"\",\n"
                "  \"InvoiceDate\": \"\",\n"
                "  \"DueDate\": \"\",\n"
                "  \"PurchaseOrderNumber\": \"\",\n"
                "  \"Company\": {\"Name\": \"\", \"Address\": \"\"},\n"
                "  \"BillTo\": {\"Name\": \"\", \"Address\": \"\"},\n"
                "  \"ShipTo\": {\"Name\": \"\", \"Address\": \"\"},\n"
                "  \"Items\": [\n"
                "    {\"Description\": \"\", \"Quantity\": 0, \"UnitPrice\": 0.0, \"Amount\": 0.0}\n"
                "  ],\n"
                "  \"Subtotal\": 0.0,\n"
                "  \"Tax\": {\"Rate\": 0.0, \"Amount\": 0.0},\n"
                "  \"TotalAmount\": 0.0,\n"
                "  \"Currency\": \"\",\n"
                "  \"PaymentTerms\": {\"DueWithinDays\": 0, \"PayableTo\": \"\"}\n"
                "}\n"
                "Rules:\n"
                "- Normalize all dates to ISO format YYYY-MM-DD.\n"
                "- If the text uses DD/MM/YYYY, convert to YYYY-MM-DD correctly.\n"
                "- Ensure numeric values are floats.\n"
                "- If a value is missing, set it to null.\n"
                "- No explanation, no markdown, no extra text.\n\n"
                f"Invoice text:\n{extracted_text}"
            )

            messages = [{"role": "user", "content": [{"text": prompt}]}]

            response = bedrock.converse(
                modelId="amazon.nova-pro-v1:0",
                messages=messages,
                inferenceConfig={"maxTokens": 1024, "temperature": 0.2, "topP": 0.9}
            )
            ai_text = response['output']['message']['content'][0]['text']
            print("🧠 Bedrock raw output:", ai_text)

            # 3. Parse JSON output
            try:
                match = re.search(r'({.*})', ai_text, re.DOTALL | re.MULTILINE)
                json_str = match.group(1) if match else ai_text.strip()
                result_json = json.loads(json_str, parse_float=Decimal)
            except Exception as e:
                print("❌ Lỗi parse JSON:", str(e))
                result_json = {"error": "Invalid JSON", "raw_output": ai_text}

            # 4. Lưu vào DynamoDB (thêm trường Tags rỗng)
            result_json['S3Object'] = f's3://{bucket}/{key}'
            if 'Tags' not in result_json:
                result_json['Tags'] = []

            table.put_item(Item=result_json)

            print("✅ Lưu vào DynamoDB thành công.")
            return make_response(200, {'message': 'Xử lý thành công', 'parsed': result_json})

        else:
            return make_response(400, {'error': 'Request không hợp lệ.'})

    except Exception as e:
        print("❌ Lỗi tổng quát:", str(e))
        return make_response(500, {'error': str(e)})
```

![Paste Code](/images/3.lambdafunctions/3.2-uploadinvoicelambda/005-sourcecode.png)

3. Nhấn **Deploy** để áp dụng thay đổi.

![Deploy](/images/3.lambdafunctions/3.2-uploadinvoicelambda/006-deploy.png)

---

#### Bước 4: Cấu hình Timeout và Memory

1. Vào tab **Configuration > General configuration**.
2. Nhấn **Edit**.

![Deploy](/images/3.lambdafunctions/3.2-uploadinvoicelambda/007-configuration.png)

3. Thay đổi các thông số sau:

    - **Memory (MB)**: `1024`
    - **Timeout**: `1 minute`

![Edit Memory Timeout](/images/3.lambdafunctions/3.2-uploadinvoicelambda/008-configtimeout.png)

4. Nhấn **Save**.

![Click Save](/images/3.lambdafunctions/3.2-uploadinvoicelambda/009-clicksave.png)

---

#### Kết luận

Bạn đã tạo thành công Lambda Function đầu tiên trong hệ thống: **UploadInvoiceFileFunction**.

-   Function này sẽ **tự động được kích hoạt** mỗi khi người dùng **tải lên hóa đơn** vào thư mục **uploads/** trong S3.
-   Nó sử dụng **Amazon Textract** để trích xuất văn bản từ hóa đơn.
-   Sau đó sử dụng **Amazon Bedrock Nova Pro** để phân tích nội dung.
-   Cuối cùng, dữ liệu sẽ được **lưu trữ vào DynamoDB** để phục vụ tra cứu và hiển thị.
