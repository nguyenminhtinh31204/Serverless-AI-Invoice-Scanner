+++
title = "Xóa CloudWatch Log Groups"
date = 2025
weight = 7
chapter = false
pre = "<b>7.7 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa các **Amazon CloudWatch Log Groups** đã được tạo trong quá trình triển khai hệ thống **Serverless AI Invoice Scanner**.

Amazon CloudWatch được sử dụng để lưu log của các dịch vụ như AWS Lambda và Amazon API Gateway. Trong quá trình kiểm thử, các log này giúp theo dõi lỗi, kiểm tra request, xem kết quả xử lý hóa đơn và debug các vấn đề như CORS, lỗi Lambda hoặc lỗi kết nối DynamoDB.

Sau khi hoàn thành bài lab, bạn nên xóa các Log Groups không còn sử dụng để tránh lưu trữ log không cần thiết và hạn chế phát sinh chi phí.

{{% notice warning %}}
Sau khi xóa CloudWatch Log Group, toàn bộ log bên trong sẽ bị xóa vĩnh viễn. Hãy đảm bảo bạn không còn cần các log này để làm minh chứng hoặc debug trước khi xóa.
{{% /notice %}}

---

#### Tài nguyên cần xóa

Trong dự án này, các CloudWatch Log Groups thường được tạo tự động cho Lambda Functions và API Gateway.

Các Log Groups Lambda có thể có dạng:

```txt
/aws/lambda/UploadInvoiceFileFunction
/aws/lambda/ProcessInvoiceFunction
/aws/lambda/InvoiceManagementFunction
```

Nếu API Gateway có bật logging, bạn cũng có thể thấy Log Groups liên quan đến API Gateway.

Ví dụ:

```txt
API-Gateway-Execution-Logs_<api-id>/dev
```

hoặc các log group có tên liên quan đến:

```txt
PostInvoiceAPI
GetInvoiceAPI
```

{{% notice info %}}
Tên Log Group thực tế có thể khác tùy theo tên Lambda Function, API Gateway và stage bạn đã tạo trong quá trình triển khai.
{{% /notice %}}

---

#### Các bước thực hiện

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **CloudWatch**.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_1.png)

- Trong thanh điều hướng bên trái, chọn **Logs**.

- Chọn **Log management**.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_2.png)

- Trong ô tìm kiếm, nhập từ khóa để tìm Log management của dự án.

Ví dụ:

```txt
UploadInvoiceFileFunction
```

hoặc:

```txt
/aws/lambda
```

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_3.png)

- Chọn Log management
 cần xóa.

Ví dụ:

```txt
/aws/lambda/UploadInvoiceFileFunction
```

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_4.png)

- Chọn **Actions**.

- Chọn **Delete log group**.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_5.png)

- Hộp thoại xác nhận xóa sẽ xuất hiện.

- Chọn **Delete** để xác nhận xóa Log Group.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_6.png)

---

#### Xóa các Log Groups còn lại

Lặp lại các bước tương tự để xóa các Log Groups còn lại của hệ thống.

Các Log Groups cần kiểm tra gồm:

```txt
/aws/lambda/UploadInvoiceFileFunction
/aws/lambda/ProcessInvoiceFunction
/aws/lambda/InvoiceManagementFunction
```

Nếu bạn có bật logging cho API Gateway, hãy kiểm tra thêm các Log Groups liên quan đến:

```txt
PostInvoiceAPI
GetInvoiceAPI
API-Gateway-Execution-Logs
```

{{% notice info %}}
Nếu bạn không bật logging cho API Gateway thì có thể không có Log Group riêng cho API Gateway. Trong trường hợp đó, bạn chỉ cần xóa các Log Groups của Lambda.
{{% /notice %}}

---

#### Kiểm tra Log Streams trước khi xóa

Trước khi xóa Log Group, bạn có thể mở Log Group để kiểm tra các **Log Streams** bên trong.

Log Streams thường chứa:

- Thời gian Lambda được gọi.
- Request từ API Gateway.
- Lỗi khi xử lý file upload.
- Lỗi khi gọi Amazon Textract.
- Lỗi khi gọi OpenAI API.
- Lỗi khi ghi dữ liệu vào DynamoDB.
- Lỗi CORS hoặc lỗi cấu hình môi trường.

Nếu cần dùng log để viết báo cáo, bạn có thể chụp màn hình hoặc copy nội dung log trước khi xóa.

---

#### Kiểm tra sau khi xóa

Sau khi xóa các Log Groups:

- Quay lại trang **Log groups** trong CloudWatch.
- Tìm lại tên Log Group vừa xóa.
- Kiểm tra rằng Log Group không còn xuất hiện.
- Nếu Lambda Function đã bị xóa, CloudWatch sẽ không tạo log mới nữa.
- Nếu Lambda Function vẫn còn tồn tại và được gọi lại, AWS có thể tự tạo lại Log Group mới cho function đó.

{{% notice warning %}}
Nếu Lambda Function vẫn còn hoạt động, CloudWatch Log Group có thể được tạo lại khi Lambda được gọi. Vì vậy, nên xóa Lambda Function trước hoặc đảm bảo không còn request gọi đến Lambda.
{{% /notice %}}

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa CloudWatch Log Groups, hệ thống sẽ mất lịch sử log liên quan đến quá trình hoạt động trước đó.

| Thành phần | Ảnh hưởng |
|---|---|
| Lambda Functions | Không còn log cũ để kiểm tra lỗi hoặc debug. |
| API Gateway | Không còn log cũ nếu API Gateway logging đã được bật. |
| React frontend | Không bị ảnh hưởng trực tiếp. |
| Amazon S3 | Không bị ảnh hưởng trực tiếp. |
| Amazon Textract | Không bị ảnh hưởng trực tiếp. |
| OpenAI API | Không bị ảnh hưởng trực tiếp, nhưng mất log backend liên quan đến request OpenAI. |
| Amazon DynamoDB | Không bị ảnh hưởng trực tiếp. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Việc xóa CloudWatch Log Groups không xóa Lambda Functions, API Gateway, S3 Bucket, DynamoDB Table, Cognito User Pool hoặc Amplify Hosting App. Đây chỉ là bước xóa dữ liệu log.
{{% /notice %}}

{{% notice warning %}}
Nếu bạn cần log để chứng minh quá trình xử lý hóa đơn trong báo cáo, hãy lưu lại ảnh chụp màn hình hoặc export log trước khi xóa.
{{% /notice %}}

{{% notice info %}}
Để tránh phát sinh log trong tương lai, hãy đảm bảo các Lambda Functions và API Gateway liên quan đã được xóa hoặc không còn được gọi.
{{% /notice %}}

---

#### Kết luận

Bạn đã xóa thành công các Amazon CloudWatch Log Groups được tạo trong hệ thống **Serverless AI Invoice Scanner**.

- Log Group của các Lambda Functions đã được xóa.
- Log liên quan đến API Gateway đã được xóa nếu có bật logging.
- Lịch sử log cũ không còn được lưu trong CloudWatch.
- Bước dọn dẹp này giúp loại bỏ dữ liệu log không còn cần thiết và hạn chế chi phí lưu trữ log trong tài khoản AWS.
