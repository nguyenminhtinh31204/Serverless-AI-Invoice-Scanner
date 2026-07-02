+++
title = "Xóa API Gateway"
date = 2025
weight = 1
chapter = false
pre = "<b>7.1 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa các tài nguyên **Amazon API Gateway** đã được tạo cho hệ thống **Serverless AI Invoice Scanner**.

Amazon API Gateway được sử dụng để cung cấp các REST API cho ứng dụng frontend. Các API này cho phép người dùng upload tệp hóa đơn, lấy dữ liệu hóa đơn, tìm kiếm hóa đơn, cập nhật tags và đánh dấu hóa đơn quan trọng.

Sau khi hoàn thành bài lab, bạn nên xóa các API Gateway không còn sử dụng để tránh giữ lại các endpoint đang hoạt động trong tài khoản AWS.

{{% notice warning %}}
Sau khi xóa API Gateway, toàn bộ routes và stages thuộc API đó sẽ bị xóa. Ứng dụng frontend sẽ không thể gọi các endpoint đã bị xóa.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, các API Gateway cần xóa bao gồm:

| Tên API | Chức năng |
|---|---|
| `PostInvoiceAPI` | Xử lý request upload hóa đơn từ frontend. |
| `GetInvoiceAPI` | Xử lý request lấy dữ liệu hóa đơn, tìm kiếm hóa đơn, cập nhật tags và đánh dấu hóa đơn quan trọng. |

Các route chính được sử dụng trong hệ thống gồm:

```txt
POST /uploads
GET /invoice
GET /invoice/{id}
GET /invoice?name=<customer_name>
PATCH /invoice/tags/{id}
PATCH /invoice/starred/{id}
```

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **API Gateway**.

![Remove API Gateway](/images/7/7.1/Screenshot_1.png)

- Trong danh sách API, chọn API dùng để upload hóa đơn:

```txt
PostInvoiceAPI
```

![Remove API Gateway](/images/7/7.1/Screenshot_2.png)

- Mở menu **API actions**.

- Chọn **Delete API**.

![Remove API Gateway](/images/7/7.1/Screenshot_3.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Nhập nội dung xác nhận:

```txt
confirm
```

![Remove API Gateway](/images/7/7.1/Screenshot_4.png)

- Chọn **Delete** để xác nhận xóa API.

- Sau khi xóa thành công, API sẽ không còn xuất hiện trong danh sách API Gateway.

![Remove API Gateway](/images/7/7.1/Screenshot_5.png)

---

#### Xóa API còn lại

Lặp lại các bước tương tự để xóa API còn lại:

```txt
GetInvoiceAPI
```

![Remove API Gateway](/images/7/7.1/Screenshot_6.png)

API này được sử dụng cho các chức năng quản lý hóa đơn như:

- Xem tất cả hóa đơn.
- Xem chi tiết hóa đơn.
- Tìm kiếm hóa đơn theo ID hoặc tên khách hàng.
- Cập nhật tags hóa đơn.
- Đánh dấu hóa đơn quan trọng.

---

#### Kiểm tra sau khi xóa

Sau khi xóa cả hai API Gateway:

- Quay lại danh sách API trong **API Gateway**.
- Kiểm tra rằng `PostInvoiceAPI` và `GetInvoiceAPI` không còn xuất hiện.
- Thử gọi lại các endpoint cũ bằng Postman hoặc frontend.
- Các endpoint đã xóa sẽ không còn truy cập được.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa API Gateway, frontend sẽ không còn giao tiếp được với các Lambda backend thông qua những endpoint đã xóa.

| Thành phần | Ảnh hưởng |
|---|---|
| React frontend | Không thể upload, tìm kiếm, xem hoặc cập nhật hóa đơn. |
| Upload Lambda | Không còn nhận request từ endpoint `POST /uploads`. |
| Invoice Management Lambda | Không còn nhận request truy vấn hoặc cập nhật hóa đơn. |
| Amazon S3 | Các file hóa đơn đã upload vẫn còn nếu chưa xóa S3 bucket. |
| Amazon DynamoDB | Dữ liệu hóa đơn vẫn còn nếu chưa xóa bảng DynamoDB. |
| Amazon Cognito | Không bị ảnh hưởng trực tiếp bởi việc xóa API Gateway. |
| Amazon CloudWatch | Log cũ của API Gateway có thể vẫn còn nếu chưa xóa riêng. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Việc xóa API Gateway không tự động xóa Lambda Functions, S3 Bucket, DynamoDB Table, Cognito User Pool hoặc CloudWatch Log Groups. Các tài nguyên này cần được xóa riêng ở các bước dọn dẹp tiếp theo.
{{% /notice %}}

{{% notice warning %}}
Nếu frontend vẫn còn lưu các URL API Gateway đã bị xóa trong biến môi trường, các request từ frontend sẽ bị lỗi. Đây là kết quả bình thường sau khi thực hiện cleanup.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công các API Gateway được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- `PostInvoiceAPI` đã được xóa.
- `GetInvoiceAPI` đã được xóa.
- Frontend không còn có thể gọi backend thông qua các API endpoint này.
- Bước dọn dẹp này giúp loại bỏ các endpoint không còn sử dụng, hạn chế request ngoài ý muốn và giảm nguy cơ phát sinh chi phí.
