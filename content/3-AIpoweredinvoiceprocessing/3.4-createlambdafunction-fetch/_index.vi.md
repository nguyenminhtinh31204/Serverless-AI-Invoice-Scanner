---
title: "Tạo Lambda Function #2"
weight: 4
chapter: false
pre: " <b> 3.4 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo Lambda Function thứ hai có tên **FetchInvoiceDetailsFunction**. Function này có nhiệm vụ truy vấn hoặc cập nhật thông tin hóa đơn trong DynamoDB, phục vụ cho các yêu cầu từ API Gateway.

---

#### Bước 1: Truy cập Lambda Console

1. Mở [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

![Open Lambda Console](/images/3.lambdafunctions/3.4-fetchinvoicelambda/001-openlambda.png)

2. Nhấn **Create function**

![Click Create Function](/images/3.lambdafunctions/3.4-fetchinvoicelambda/002-createfunction.png)

---

#### Bước 2: Cấu hình Lambda Function

1. Trong phần **Author from scratch**, nhập các thông tin sau:

    - **Function name**: `FetchInvoiceDetailsFunction`
    - **Runtime**: `Python 3.12`
    - **Architecture**: `x86_64`
    - **Permissions**: Chọn **Use an existing role**
    - **Existing role**: `LambdaExecutionRole-AIInvoiceScanner`

![Configure Function](/images/3.lambdafunctions/3.4-fetchinvoicelambda/003-configurefunction.png)

2. Nhấn **Create function**

![Finish Creating Function](/images/3.lambdafunctions/3.4-fetchinvoicelambda/004-finishcreate.png)

---

#### Bước 3: Thêm mã nguồn Python

1. Cuộn xuống phần **Code**, thay nội dung mặc định bằng đoạn mã Python sau:

```python
import boto3
import json
import os
import re
from decimal import Decimal
from boto3.dynamodb.conditions import Key, Attr

# ============================================================
# CONFIG
# ============================================================
dynamodb = boto3.resource("dynamodb")

DYNAMO_TABLE_NAME = os.environ.get("DYNAMO_TABLE_NAME", "InvoiceData")
table = dynamodb.Table(DYNAMO_TABLE_NAME)

STARRED_INDEX_NAME = os.environ.get("STARRED_INDEX_NAME", "StarredInvoicesIndex")
CUSTOMER_NAME_INDEX_NAME = os.environ.get("CUSTOMER_NAME_INDEX_NAME", "CustomerName-index")


# ============================================================
# RESPONSE HELPERS
# ============================================================
def decimal_default(obj):
    if isinstance(obj, Decimal):
        # Nếu là số nguyên thì trả int cho đẹp, ngược lại trả float
        if obj % 1 == 0:
            return int(obj)
        return float(obj)
    raise TypeError(f"Object of type {type(obj).__name__} is not JSON serializable")


def make_headers():
    return {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Credentials": "true",
        "Access-Control-Allow-Headers": "Content-Type,Authorization,X-Amz-Date,X-Api-Key,X-Amz-Security-Token",
        "Access-Control-Allow-Methods": "OPTIONS,GET,PATCH"
    }


def make_response(status_code, body):
    return {
        "statusCode": status_code,
        "headers": make_headers(),
        "body": json.dumps(body, default=decimal_default, ensure_ascii=False)
    }


def parse_body(event):
    body = event.get("body")
    if not body:
        return None

    if isinstance(body, dict):
        return body

    try:
        return json.loads(body)
    except json.JSONDecodeError:
        return None


def normalize_item_for_frontend(item):
    """
    Chuẩn hóa dữ liệu trả về cho frontend.
    - Hỗ trợ field top-level và ExtractedData.
    - Hỗ trợ cả Tags và tags.
    - Trả về cả Tags và tags để frontend đọc kiểu nào cũng được.
    """
    if not item:
        return item

    extracted = item.get("ExtractedData") or {}

    for field in ["InvoiceNumber", "CustomerName", "InvoiceDate", "TotalAmount"]:
        if item.get(field) is None and isinstance(extracted, dict) and extracted.get(field) is not None:
            item[field] = extracted.get(field)

    if item.get("Starred") is None:
        item["Starred"] = "false"

    # Lấy tag từ Tags trước, nếu không có thì lấy từ tags
    tags = item.get("Tags")

    if tags is None:
        tags = item.get("tags")

    if tags is None:
        tags = []

    # Nếu lỡ lưu dạng string thì chuyển thành list
    if isinstance(tags, str):
        tags = [tags]

    # Nếu dữ liệu lỗi không phải list thì ép về []
    if not isinstance(tags, list):
        tags = []

    # Chuẩn hóa lại tag: chỉ giữ string, trim, bỏ rỗng, bỏ trùng
    clean_tags = []
    seen = set()

    for tag in tags:
        if isinstance(tag, str):
            clean_tag = tag.strip()
            key = clean_tag.lower()

            if clean_tag and key not in seen:
                clean_tags.append(clean_tag)
                seen.add(key)

    # Trả cả 2 field cho frontend
    item["Tags"] = clean_tags
    item["tags"] = clean_tags

    return item


# ============================================================
# DYNAMODB PAGINATION HELPERS
# ============================================================
def scan_all(**kwargs):
    items = []
    response = table.scan(**kwargs)
    items.extend(response.get("Items", []))

    while "LastEvaluatedKey" in response:
        response = table.scan(
            ExclusiveStartKey=response["LastEvaluatedKey"],
            **kwargs
        )
        items.extend(response.get("Items", []))

    return items


def query_all(**kwargs):
    items = []
    response = table.query(**kwargs)
    items.extend(response.get("Items", []))

    while "LastEvaluatedKey" in response:
        response = table.query(
            ExclusiveStartKey=response["LastEvaluatedKey"],
            **kwargs
        )
        items.extend(response.get("Items", []))

    return items


# ============================================================
# ROUTE HELPERS
# ============================================================
def get_route_info(event):
    http_method = (
        event.get("httpMethod")
        or event.get("requestContext", {}).get("http", {}).get("method")
        or ""
    ).upper()

    resource = event.get("resource") or event.get("routeKey") or ""
    path = event.get("path") or event.get("rawPath") or ""
    path_params = event.get("pathParameters") or {}

    invoice_id = path_params.get("id")

    # Fallback nếu pathParameters chưa map đúng
    if not invoice_id and path:
        patterns = [
            r"^/invoice/starred/([^/]+)$",
            r"^/invoice/tags/([^/]+)$",
            r"^/invoice/([^/]+)$"
        ]
        for pattern in patterns:
            match = re.match(pattern, path)
            if match:
                invoice_id = match.group(1)
                break

    return http_method, resource, path, invoice_id


def is_route(resource, path, expected_resource, expected_path_regex=None):
    if resource == expected_resource:
        return True

    if expected_path_regex and path and re.match(expected_path_regex, path):
        return True

    return False


# ============================================================
# HANDLER
# ============================================================
def lambda_handler(event, context):
    # Không log toàn bộ event nếu có Authorization header/token
    print("DEBUG EVENT KEYS:", list(event.keys()))

    try:
        http_method, resource, path, invoice_id = get_route_info(event)
        query_params = event.get("queryStringParameters") or {}

        # CORS preflight
        if http_method == "OPTIONS":
            return make_response(200, {"message": "OK"})

        # ========================================================
        # PATCH /invoice/starred/{id}
        # Body: {"starred": true}
        # Lưu Starred dạng string "true"/"false" để dùng làm GSI key.
        # ========================================================
        if (
            http_method == "PATCH"
            and invoice_id
            and is_route(resource, path, "/invoice/starred/{id}", r"^/invoice/starred/[^/]+$")
        ):
            body = parse_body(event)
            if body is None:
                return make_response(400, {"error": "Body must be valid JSON and must not be empty"})

            starred = body.get("starred")
            if not isinstance(starred, bool):
                return make_response(400, {"error": "The 'starred' field must be a boolean"})

            existing = table.get_item(Key={"InvoiceId": invoice_id})
            if "Item" not in existing:
                return make_response(404, {"error": f"Invoice with the given ID was not found: {invoice_id}."})

            starred_str = "true" if starred else "false"

            response = table.update_item(
                Key={"InvoiceId": invoice_id},
                UpdateExpression="SET Starred = :starred",
                ExpressionAttributeValues={":starred": starred_str},
                ReturnValues="ALL_NEW"
            )

            item = normalize_item_for_frontend(response.get("Attributes", {}))

            return make_response(200, {
                "message": "Starred updated successfully",
                "InvoiceId": invoice_id,
                "Starred": starred_str,
                "data": item
            })

        # ========================================================
        # PATCH /invoice/tags/{id}
        # Body:
        #   {"tags": ["VIP", "Urgent"]}
        # hoặc:
        #   {"Tags": ["VIP", "Urgent"]}
        #
        # Mặc định: add thêm tag, không ghi đè tag cũ.
        # Nếu muốn ghi đè toàn bộ:
        #   {"tags": ["VIP"], "mode": "replace"}
        # ========================================================
        if (
            http_method == "PATCH"
            and invoice_id
            and is_route(resource, path, "/invoice/tags/{id}", r"^/invoice/tags/[^/]+$")
        ):
            body = parse_body(event)
            if body is None:
                return make_response(400, {"error": "Body must be valid JSON and must not be empty"})

            # Hỗ trợ cả tags và Tags
            incoming_tags = body.get("tags")
            if incoming_tags is None:
                incoming_tags = body.get("Tags")

            if not isinstance(incoming_tags, list):
                return make_response(400, {"error": "Tags must be an array"})

            # Làm sạch tags gửi lên
            clean_new_tags = []
            seen_new = set()

            for tag in incoming_tags:
                if not isinstance(tag, str):
                    return make_response(400, {"error": "Every tag must be a string"})

                clean_tag = tag.strip()
                key = clean_tag.lower()

                if clean_tag and key not in seen_new:
                    clean_new_tags.append(clean_tag)
                    seen_new.add(key)

            existing = table.get_item(Key={"InvoiceId": invoice_id})
            if "Item" not in existing:
                return make_response(404, {"error": f"Invoice with the given ID was not found: {invoice_id}."})

            existing_item = existing["Item"]

            # Lấy tags cũ từ Tags hoặc tags
            old_tags = existing_item.get("Tags")
            if old_tags is None:
                old_tags = existing_item.get("tags")

            if old_tags is None:
                old_tags = []

            if isinstance(old_tags, str):
                old_tags = [old_tags]

            if not isinstance(old_tags, list):
                old_tags = []

            mode = body.get("mode", "append")

            # Nếu mode = replace thì thay toàn bộ tag
            if mode == "replace":
                final_tags = clean_new_tags
            else:
                # Mặc định append: gộp tag cũ + tag mới, chống trùng
                final_tags = []
                seen_all = set()

                for tag in old_tags + clean_new_tags:
                    if isinstance(tag, str):
                        clean_tag = tag.strip()
                        key = clean_tag.lower()

                        if clean_tag and key not in seen_all:
                            final_tags.append(clean_tag)
                            seen_all.add(key)

            response = table.update_item(
                Key={"InvoiceId": invoice_id},
                UpdateExpression="SET Tags = :tags",
                ExpressionAttributeValues={
                    ":tags": final_tags
                },
                ReturnValues="ALL_NEW"
            )

            item = normalize_item_for_frontend(response.get("Attributes", {}))

            return make_response(200, {
                "message": "Tags updated successfully",
                "InvoiceId": invoice_id,
                "Tags": final_tags,
                "tags": final_tags,
                "data": item
            })

        # ========================================================
        # GET /invoice/starred
        # Cần GSI:
        # IndexName = StarredInvoicesIndex
        # Partition key = Starred, String
        # ========================================================
        if (
            http_method == "GET"
            and is_route(resource, path, "/invoice/starred", r"^/invoice/starred$")
        ):
            try:
                items = query_all(
                    IndexName=STARRED_INDEX_NAME,
                    KeyConditionExpression=Key("IsStarred").eq("true")
                )
            except Exception as e:
                return make_response(500, {
                    "error": f"Error querying {STARRED_INDEX_NAME}: {str(e)}",
                    "hint": "Check that the GSI exists and has partition key Starred with type String."
                })

            items = [normalize_item_for_frontend(item) for item in items]
            return make_response(200, items)

        # ========================================================
        # GET /invoice/{id}
        # ========================================================
        if http_method == "GET" and invoice_id:
            response = table.get_item(Key={"InvoiceId": invoice_id})

            if "Item" not in response:
                return make_response(404, {"error": f"Invoice with the given ID was not found: {invoice_id}."})

            item = normalize_item_for_frontend(response["Item"])
            return make_response(200, item)

        # ========================================================
        # GET /invoice?name=...
        # Tìm theo CustomerName.
        # Hỗ trợ cả:
        #   1. CustomerName top-level
        #   2. ExtractedData.CustomerName
        # ========================================================
        if http_method == "GET" and query_params.get("name"):
            name = query_params["name"].strip()

            if not name:
                return make_response(400, {"error": "Query parameter 'name' must not be empty"})

            items = []

            # Cách 1: Thử query GSI CustomerName-index nếu có CustomerName top-level
            try:
                items = query_all(
                    IndexName=CUSTOMER_NAME_INDEX_NAME,
                    KeyConditionExpression=Key("CustomerName").eq(name)
                )
            except Exception as e:
                print(f"⚠️ Query {CUSTOMER_NAME_INDEX_NAME} failed:", str(e))
                items = []

            # Cách 2: Nếu GSI không có kết quả, fallback sang scan
            # Dùng cho dữ liệu CustomerName nằm trong ExtractedData.CustomerName
            if not items:
                try:
                    items = scan_all(
                        FilterExpression=(
                            Attr("CustomerName").eq(name) |
                            Attr("ExtractedData.CustomerName").eq(name)
                        )
                    )
                except Exception as scan_error:
                    return make_response(500, {
                        "error": f"Error searching by CustomerName: {str(scan_error)}",
                        "hint": "CustomerName may be stored inside ExtractedData.CustomerName. Consider saving CustomerName as a top-level field for better search."
                    })

            items = [normalize_item_for_frontend(item) for item in items]

            if not items:
                return make_response(404, {
                    "error": f"No invoice found with the customer name: {name}.",
                    "hint": "Search is exact and case-sensitive. Try the exact value stored in DynamoDB, for example: John Smith"
                })

            return make_response(200, items)

        # ========================================================
        # GET /invoice
        # Lấy tất cả hóa đơn, có phân trang nội bộ để không mất dữ liệu.
        # ========================================================
        if http_method == "GET":
            items = scan_all()
            items = [normalize_item_for_frontend(item) for item in items]
            return make_response(200, items)

        return make_response(400, {"error": "Method not supported"})

    except Exception as e:
        print("ERROR:", str(e))
        return make_response(500, {"error": str(e)})
```

![Paste Source Code](/images/3.lambdafunctions/3.4-fetchinvoicelambda/005-code.png)

2.  Nhấn **Deploy** để lưu và áp dụng đoạn mã nguồn.

![Deploy](/images/3.lambdafunctions/3.4-fetchinvoicelambda/006-deploy.png)

---

#### Bước 4: Cấu hình Timeout và Memory

1.  Vào **Configuration > General configuration**, nhấn **Edit**.

![Edit General Configuration](/images/3.lambdafunctions/3.4-fetchinvoicelambda/007-editconfig.png)

2.  Cấu hình thông tin như sau:

    -   **Description**: `Retrieves invoice data from DynamoDB based on invoice ID via API Gateway request`
    -   **Memory (MB)**: 128
    -   **Timeout**: 3 seconds

![Set Memory and Timeout](/images/3.lambdafunctions/3.4-fetchinvoicelambda/008-settimeout.png)

3. **Configuration -> Environment variables**, click **Edit**,

![Set Environment variables](/images/3.lambdafunctions/3.4-fetchinvoicelambda/010-more.png)

4.  Nhấn **Save**.

![Click Save](/images/3.lambdafunctions/3.4-fetchinvoicelambda/009-clicksave%20%282%29.png)

---

#### Kết luận

Bạn đã tạo xong Lambda Function **FetchInvoiceDetailsFunction**, dùng để xử lý việc truy vấn và cập nhật dữ liệu hóa đơn từ DynamoDB. Function này sẽ được kết nối với API Gateway để phục vụ các yêu cầu từ phía frontend.
