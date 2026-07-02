+++
title = "Xóa Lambda Functions"
date = 2025
weight = 2
chapter = false
pre = "<b>7.2 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa các **AWS Lambda Functions** đã được tạo cho hệ thống **Serverless AI Invoice Scanner**.

AWS Lambda được sử dụng để xử lý logic backend của hệ thống, bao gồm nhận file upload từ frontend, lưu file vào Amazon S3, xử lý hóa đơn bằng Amazon Textract, gọi OpenAI API để chuẩn hóa dữ liệu và quản lý dữ liệu hóa đơn trong Amazon DynamoDB.

Sau khi hoàn thành bài lab, bạn nên xóa các Lambda Functions không còn sử dụng để tránh giữ lại tài nguyên backend không cần thiết.

{{% notice warning %}}
Sau khi xóa Lambda Function, các API hoặc trigger đang liên kết với function đó sẽ không thể thực thi được nữa. Hãy đảm bảo bạn đã xóa hoặc không còn sử dụng API Gateway, S3 trigger và các tài nguyên liên quan trước khi xóa Lambda.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, các Lambda Functions cần xóa bao gồm:

| Tên Lambda Function | Chức năng |
|---|---|
| `UploadInvoiceFileFunction` | Nhận file hóa đơn từ frontend thông qua API Gateway và upload vào Amazon S3. |
| `ProcessInvoiceFunction` | Được kích hoạt sau khi file được upload vào S3, dùng Amazon Textract và OpenAI API để xử lý hóa đơn. |
| `InvoiceManagementFunction` | Xử lý các chức năng lấy danh sách hóa đơn, xem chi tiết, tìm kiếm, cập nhật tags và đánh dấu hóa đơn quan trọng. |

{{% notice info %}}
Tên Lambda Function trong tài khoản AWS của bạn có thể khác một chút so với bảng trên. Hãy chọn đúng các Lambda đã tạo cho hệ thống Serverless AI Invoice Scanner.
{{% /notice %}}

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **Lambda**.

![Remove Lambda Functions](/images/7/7.2/Screenshot_1.png)

- Trong giao diện Lambda, chọn **Functions** ở thanh điều hướng bên trái.

![Remove Lambda Functions](/images/7/7.2/Screenshot_2.png)

- Trong danh sách Lambda Functions, tìm function cần xóa, ví dụ:

```txt
UploadInvoiceFileFunction
```

![Remove Lambda Functions](/images/7/7.2/Screenshot_3.png)

- Chọn Lambda Function đó để mở trang chi tiết.

- Kiểm tra lại tên function, runtime và các trigger liên quan để đảm bảo bạn đang xóa đúng function.

![Remove Lambda Functions](/images/7/7.2/Screenshot_4.png)

- Chọn menu **Actions**.

- Chọn **Delete function**.

![Remove Lambda Functions](/images/7/7.2/Screenshot_6.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Nhập nội dung xác nhận theo yêu cầu của AWS Console, thường là:

```txt
delete
```

hoặc nhập đúng tên Lambda Function.

![Remove Lambda Functions](/images/7/7.2/Screenshot_5.png)

- Chọn **Delete** để xác nhận xóa Lambda Function.

---

#### Xóa các Lambda còn lại

Lặp lại các bước tương tự để xóa các Lambda Functions còn lại trong hệ thống:

```txt
FetchInvoiceDetailsFunction
```

Các function này tương ứng với các phần xử lý chính của hệ thống:

- `UploadInvoiceFileFunction`: xử lý upload file hóa đơn.
- `FetchInvoiceDetailsFunction`: xử lý file hóa đơn sau khi được lưu vào S3,xử lý truy vấn và cập nhật dữ liệu hóa đơn.

---

#### Kiểm tra sau khi xóa

Sau khi xóa các Lambda Functions:

- Quay lại danh sách **Functions** trong AWS Lambda.
- Kiểm tra rằng các function của dự án không còn xuất hiện trong danh sách.
- Nếu thử gọi API Gateway hoặc S3 trigger cũ, hệ thống sẽ không còn Lambda để xử lý request.
- Kiểm tra lại CloudWatch Logs nếu cần xác nhận function không còn được gọi.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa Lambda Functions, các phần backend của hệ thống sẽ không còn hoạt động.

| Thành phần | Ảnh hưởng |
|---|---|
| API Gateway | Không thể gọi backend nếu Lambda tích hợp đã bị xóa. |
| React frontend | Các chức năng upload, xem, tìm kiếm, cập nhật hóa đơn sẽ bị lỗi. |
| Amazon S3 | File hóa đơn đã upload vẫn còn nếu bucket chưa bị xóa. |
| S3 Event Trigger | Không còn Lambda để xử lý file mới upload. |
| Amazon Textract | Không còn được gọi từ Lambda xử lý hóa đơn. |
| OpenAI API | Không còn được gọi từ Lambda backend. |
| Amazon DynamoDB | Dữ liệu hóa đơn vẫn còn nếu bảng chưa bị xóa. |
| Amazon CloudWatch | Log cũ của Lambda có thể vẫn còn nếu chưa xóa Log Groups. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Việc xóa Lambda Function không tự động xóa CloudWatch Log Groups tương ứng. Nếu muốn dọn dẹp hoàn toàn, bạn cần xóa các Log Groups của Lambda ở bước cleanup CloudWatch.
{{% /notice %}}

{{% notice warning %}}
Nếu Lambda Function có lưu biến môi trường như OpenAI API Key, việc xóa function sẽ loại bỏ cấu hình đó khỏi Lambda. Tuy nhiên, nếu bạn lưu API key trong AWS Secrets Manager, cần xóa secret riêng ở bước cleanup tài nguyên tùy chọn.
{{% /notice %}}

{{% notice info %}}
Nếu Lambda Function đang được API Gateway hoặc S3 Bucket sử dụng, bạn nên xóa API Gateway và S3 trigger trước hoặc sau đó kiểm tra lại để đảm bảo không còn liên kết không hợp lệ.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công các AWS Lambda Functions được sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- Lambda upload hóa đơn đã được xóa.
- Lambda xử lý hóa đơn bằng Textract và OpenAI API đã được xóa.
- Lambda quản lý dữ liệu hóa đơn đã được xóa.
- Backend serverless không còn xử lý request từ API Gateway hoặc S3 trigger.
- Bước dọn dẹp này giúp loại bỏ các tài nguyên tính toán không còn sử dụng trong tài khoản AWS.
