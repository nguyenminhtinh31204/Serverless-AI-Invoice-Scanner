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
from decimal import Decimal
from boto3.dynamodb.conditions import Key

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('InvoiceData')

def decimal_default(obj):
    if isinstance(obj, Decimal):
        return float(obj)

def lambda_handler(event, context):
    print("DEBUG EVENT:", json.dumps(event))

    headers = {
        "Access-Control-Allow-Origin": "*",
        "Access-Control-Allow-Credentials": True,
        "Access-Control-Allow-Headers": "*",
        "Access-Control-Allow-Methods": "OPTIONS,GET,PATCH"
    }

    try:
        http_method = event.get("httpMethod", "")
        resource = event.get("resource", "")
        path_params = event.get("pathParameters") or {}
        invoice_id = path_params.get("id")

        # === PATCH /invoice/starred/{id} ===
        if http_method == "PATCH" and invoice_id and resource == "/invoice/starred/{id}":
            if not event.get("body"):
                return {
                    'statusCode': 400,
                    'headers': headers,
                    'body': json.dumps({'error': 'Body không được để trống'})
                }

            # Kiểm tra tồn tại invoice
            existing = table.get_item(Key={'InvoiceId': invoice_id})
            if 'Item' not in existing:
                return {
                    'statusCode': 404,
                    'headers': headers,
                    'body': json.dumps({'error': f'Không tìm thấy hóa đơn với ID {invoice_id}.'})
                }

            body = json.loads(event["body"])
            starred = body.get("starred")
            if not isinstance(starred, bool):
                return {
                    'statusCode': 400,
                    'headers': headers,
                    'body': json.dumps({'error': 'Trường starred phải là boolean'})
                }

            starred_str = "true" if starred else "false"

            table.update_item(
                Key={'InvoiceId': invoice_id},
                UpdateExpression="SET Starred = :starred",
                ExpressionAttributeValues={':starred': starred_str}
            )

            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps({'message': 'Cập nhật Starred thành công', 'InvoiceId': invoice_id, 'Starred': starred_str})
            }

        # === PATCH /invoice/tags/{id} ===
        if http_method == "PATCH" and invoice_id and resource == "/invoice/tags/{id}":
            if not event.get("body"):
                return {
                    'statusCode': 400,
                    'headers': headers,
                    'body': json.dumps({'error': 'Body không được để trống'})
                }

            # Kiểm tra tồn tại invoice
            existing = table.get_item(Key={'InvoiceId': invoice_id})
            if 'Item' not in existing:
                return {
                    'statusCode': 404,
                    'headers': headers,
                    'body': json.dumps({'error': f'Không tìm thấy hóa đơn với ID {invoice_id}.'})
                }

            body = json.loads(event["body"])
            tags = body.get("tags")
            if not isinstance(tags, list):
                return {
                    'statusCode': 400,
                    'headers': headers,
                    'body': json.dumps({'error': 'Tags phải là một mảng'})
                }

            table.update_item(
                Key={'InvoiceId': invoice_id},
                UpdateExpression="SET Tags = :tags",
                ExpressionAttributeValues={':tags': tags}
            )

            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps({'message': 'Cập nhật Tags thành công', 'InvoiceId': invoice_id, 'Tags': tags})
            }

        # === GET /invoice/starred ===
        if http_method == "GET" and resource == "/invoice/starred":
            try:
                response = table.query(
                    IndexName="StarredInvoicesIndex",
                    KeyConditionExpression=Key("IsStarred").eq("true")
                )
                items = response.get("Items", [])
                return {
                    'statusCode': 200,
                    'headers': headers,
                    'body': json.dumps(items, default=decimal_default)
                }
            except Exception as e:
                return {
                    'statusCode': 500,
                    'headers': headers,
                    'body': json.dumps({'error': f'Lỗi truy vấn StarredInvoicesIndex: {str(e)}'})
                }

        # === GET /invoice/{id} ===
        if invoice_id and http_method == "GET":
            response = table.get_item(Key={'InvoiceId': invoice_id})
            if 'Item' not in response:
                return {
                    'statusCode': 404,
                    'headers': headers,
                    'body': json.dumps({'error': f'Không tìm thấy hóa đơn với ID {invoice_id}.'})
                }
            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps(response['Item'], default=decimal_default)
            }

        # === GET /invoice?name=... ===
        if http_method == "GET" and event.get('queryStringParameters') and event['queryStringParameters'].get('name'):
            name = event['queryStringParameters']['name']
            try:
                response = table.query(
                    IndexName='CustomerName-index',
                    KeyConditionExpression=Key('CustomerName').eq(name)
                )
                items = response.get('Items', [])
            except Exception as e:
                return {
                    'statusCode': 500,
                    'headers': headers,
                    'body': json.dumps({'error': f'Lỗi truy vấn CustomerName: {str(e)}'})
                }

            if not items:
                return {
                    'statusCode': 404,
                    'headers': headers,
                    'body': json.dumps({'error': f'Không tìm thấy hóa đơn với tên khách hàng {name}.'})
                }

            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps(items, default=decimal_default)
            }

        # === GET /invoice (tất cả) ===
        if http_method == "GET":
            response = table.scan()
            items = response.get('Items', [])
            return {
                'statusCode': 200,
                'headers': headers,
                'body': json.dumps(items, default=decimal_default)
            }

        return {
            'statusCode': 400,
            'headers': headers,
            'body': json.dumps({'error': 'Phương thức không được hỗ trợ'})
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'headers': headers,
            'body': json.dumps({'error': str(e)})
        }
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

3.  Nhấn **Save**.

![Click Save](/images/3.lambdafunctions/3.4-fetchinvoicelambda/009-clicksave.png)

---

#### Kết luận

Bạn đã tạo xong Lambda Function **FetchInvoiceDetailsFunction**, dùng để xử lý việc truy vấn và cập nhật dữ liệu hóa đơn từ DynamoDB. Function này sẽ được kết nối với API Gateway để phục vụ các yêu cầu từ phía frontend.
