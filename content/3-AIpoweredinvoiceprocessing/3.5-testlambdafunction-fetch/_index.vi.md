---
title: "Kiểm thử Lambda Function #2"
weight: 5
chapter: false
pre: " <b> 3.5 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ thực hiện kiểm thử Lambda Function **FetchInvoiceDetailsFunction**. Function này có nhiệm vụ đọc và cập nhật thông tin hóa đơn từ DynamoDB thông qua các API endpoint như GET hoặc PATCH. Kiểm thử sẽ giúp xác minh rằng Lambda hoạt động đúng khi nhận được input từ API Gateway.

{{% notice warning %}}
⚠️ Đảm bảo bạn đã có ít nhất một file hóa đơn trong S3 Bucket và bản ghi tương ứng trong bảng DynamoDB **InvoiceData** trước khi bắt đầu kiểm thử.
{{% /notice %}}

#### Bước 1: Tạo Test Event cho truy xuất dữ liệu

1.  Truy cập **AWS Lambda Console**.

![Open Lambda Console](/images/3.lambdafunctions/3.5-testfetch/001-openlambda.png)

2.  Chọn function **FetchInvoiceDetailsFunction**.

![Lambda Function](/images/3.lambdafunctions/3.5-testfetch/002-selectfunction.png)

3.  Chuyển sang tab **Test**.

4.  Kéo xuống đến phần **Test event** và cấu hình như sau:

    -   **Event name**: `TestGetInvoice`
    -   **Template**: Hello World

![Test event](/images/3.lambdafunctions/3.5-testfetch/004-createevent.png)

5.  Dán nội dung JSON sau vào phần event:

    ```json
    {
        "httpMethod": "GET",
        "path": "/invoice/demo_invoice.png",
        "pathParameters": {
            "id": "InvoiceId_của_bạn"
        }
    }
    ```

> 📌 Thay giá trị của `"id"` bằng một **InvoiceId hợp lệ** đã tồn tại trong bảng DynamoDB **InvoiceData**.

![JSON](/images/3.lambdafunctions/3.5-testfetch/005-pastejson.png)

1. Cuộn lên và nhấn **Save**.

![Save](/images/3.lambdafunctions/3.5-testfetch/006-saveevent.png)

---

#### Bước 2: Chạy kiểm thử

1.  Sau khi tạo xong Test Event, nhấn nút **Test** để chạy.

![Test](/images/3.lambdafunctions/3.5-testfetch/007-test.png)

2.  Quan sát phần **Execution results** được hiển thị sau khi chạy:

    -   Nếu chạy thành công, bạn sẽ thấy dòng: **Status: succeeded** cùng với log output hiển thị nội dung xử lý.

![Execution results](/images/3.lambdafunctions/3.5-testfetch/008-executionresult.png)
