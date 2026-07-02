---
title: "Kiểm thử Lambda Function #1"
weight: 3
chapter: false
pre: " <b> 3.3 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ thực hiện kiểm thử Lambda Function **UploadInvoiceFileFunction** bằng cách tải lên một tệp hóa đơn mẫu vào S3 bucket. Việc kiểm thử nhằm xác nhận toàn bộ quá trình xử lý, từ khi người dùng tải file lên S3, trích xuất dữ liệu bằng Textract, phân tích bằng Bedrock, đến khi lưu dữ liệu vào DynamoDB, hoạt động chính xác.

---

#### Yêu cầu trước khi kiểm thử

Trước khi thực hiện kiểm thử Lambda Function, bạn cần chuẩn bị tệp hóa đơn mẫu để tải lên. Vui lòng tải tệp sau về máy của bạn:

-   [Tải file mẫu demo_invoice.png](/images/demo_invoice.png)

{{% notice info %}}
🔧 **Ghi chú**: Nếu bạn sử dụng tệp hóa đơn khác, hãy đổi tên thành **demo_invoice.png** trước khi tải lên thư mục **uploads/** trong S3.
{{% /notice %}}

---

#### Bước 1: Tải tệp hóa đơn mẫu vào S3 Bucket

1. Truy cập **Amazon S3 Console**.

![Amazon S3 Console](/images/3.lambdafunctions/3.3-testupload/001-s3console.png)

2. Vào bucket có tên: **invoice-upload-s3-bucket**.

![Access S3 Bucket](/images/3.lambdafunctions/3.3-testupload/002-accesss3bucket.png)

3. Mở thư mục **uploads/**.

![Open Folder](/images/3.lambdafunctions/3.3-testupload/003-openfolder.png)

4. Nhấn **Upload**.

![Click Upload](/images/3.lambdafunctions/3.3-testupload/004-clickupload.png)

5. Nhấn **Add files**.

![Click Add Files](/images/3.lambdafunctions/3.3-testupload/005-clickaddfiles.png)

6. Chọn file mẫu: **demo_invoice.png**.

![Choose File](/images/3.lambdafunctions/3.3-testupload/006-demo-invoice.png)

7. Nhấn **Upload** để tải lên.

![Upload file to S3](/images/3.lambdafunctions/3.3-testupload/007-uploadfiletos3.png)

8. Kiểm tra sau khi tải tệp.

![Check file](/images/3.lambdafunctions/3.3-testupload/008-uploadfilesuccess.png)

---

#### Bước 2: Tạo Test Event trong Lambda Console

1. Mở **AWS Lambda Console**.

![AWS Lambda Console](/images/3.lambdafunctions/3.3-testupload/009-lambdaconsole.png)

2. Truy cập function **UploadInvoiceFileFunction**.

3. Nhấn tab **Test** để tạo một Test Event mới.

4. **Event name**: `TestUploadInvoice`.

![Test event](/images/3.lambdafunctions/3.3-testupload/010-testevent.png)

5. Dán nội dung JSON sau vào phần event:

```json
{
  "Records": [
    {
      "eventVersion": "2.1",
      "eventSource": "aws:s3",
      "awsRegion": "ap-southeast-1",
      "eventTime": "2025-07-31T12:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "s3": {
        "bucket": {
          "name": "invoice-upload-s3-bucket-113"
        },
        "object": {
          "key": "uploads/demo_invoice.png"
        }
      }
    }
  ]
}

```

![Paste JSON](/images/3.lambdafunctions/3.3-testupload/011-json.png)

6. Nhấn **Save**.

![Save](/images/3.lambdafunctions/3.3-testupload/012-save.png)

---

#### Bước 3: Chạy kiểm thử

1.  Sau khi tạo xong Test Event, nhấn nút **Test** để chạy.

![Test event](/images/3.lambdafunctions/3.3-testupload/013-test.png)

2.  Quan sát phần **Execution results** được hiển thị sau khi chạy:

    -   Nếu chạy thành công, bạn sẽ thấy dòng: **Status: succeeded** cùng với log output hiển thị nội dung xử lý.

![Execution function](/images/3.lambdafunctions/3.3-testupload/014-executionfunction.png)

#### Bước 4: Kiểm tra log chi tiết trên CloudWatch

1.  Trong Lambda Console, chọn tab **Monitor**.

![Tab Monitor](/images/3.lambdafunctions/3.3-testupload/015-tabmonitor.png)

2.  Nhấn **View CloudWatch logs**.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/016-viewcloudwatchlogs.png)

3.  Mở **log stream mới nhất**.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/017-viewcloudwatchlogs.png)

4.  Xem logs event.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/018-checklogs.png)

---

#### Bước 5: Kiểm tra dữ liệu trong DynamoDB

1.  Truy cập **AWS DynamoDB Console**.

![AWS DynamoDB Console](/images/3.lambdafunctions/3.3-testupload/019-dynamodbconsole.png)

2.  Vào bảng **InvoiceData**.

![InvoiceData](/images/3.lambdafunctions/3.3-testupload/020-invoicedata.png)

3.  Nhấn **Explore table items**.

![Explore table items](/images/3.lambdafunctions/3.3-testupload/021-exploretableitems.png)

4.  Tìm bản ghi có **InvoiceId** tương ứng với file **demo_invoice.png** vừa tải lên để xác nhận dữ liệu đã được lưu thành công.

![Explore table items](/images/3.lambdafunctions/3.3-testupload/022-exploretableitems.png)

{{% notice warning %}}
⚠️ **Lưu ý**: Đảm bảo tất cả tài nguyên (Lambda, S3, DynamoDB, Textract và Bedrock) đều được tạo trong cùng một Region: **Singapore (ap-southeast-1)** để đảm bảo hệ thống hoạt động đồng bộ.
{{% /notice %}}
