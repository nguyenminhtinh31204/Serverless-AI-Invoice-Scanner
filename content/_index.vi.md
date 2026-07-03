---
title: "Xây dựng hệ thống quét hóa đơn bằng AI trên kiến trúc Serverless"
weight: 1
chapter: false
---

# Xây dựng hệ thống quét hóa đơn bằng AI trên kiến trúc Serverless

### Tổng quan

Trong bài lab này, bạn sẽ xây dựng một hệ thống quét hóa đơn sử dụng AI trên kiến trúc serverless của AWS. Ứng dụng cho phép người dùng tải lên file hóa đơn, lưu trữ file trong Amazon S3, trích xuất văn bản và dữ liệu có cấu trúc bằng Amazon Textract, chuẩn hóa dữ liệu đã trích xuất bằng OpenAI API, lưu kết quả cuối cùng vào Amazon DynamoDB và truy cập dữ liệu thông qua Amazon API Gateway.

Phần frontend được triển khai bằng AWS Amplify Hosting và sử dụng Amazon Cognito để hỗ trợ chức năng đăng ký, đăng nhập người dùng. Hệ thống cũng sử dụng Amazon CloudWatch để theo dõi log và hỗ trợ xử lý lỗi trong quá trình vận hành backend.

{{% notice info %}}
Trong project này, bước chuẩn hóa dữ liệu bằng AI sử dụng OpenAI API thay cho Amazon Bedrock. OpenAI API key cần được lưu trữ an toàn ở phía backend, ví dụ trong Lambda environment variables hoặc AWS Secrets Manager, và không được đưa trực tiếp vào frontend.
{{% /notice %}}

![Architecture Diagram](/images/architecture-log.png)

---

#### Amazon S3

Amazon S3 (Simple Storage Service) là dịch vụ lưu trữ đối tượng của AWS, được sử dụng để lưu các file hóa đơn do người dùng tải lên. Trong hệ thống này, file hóa đơn thường được lưu trong S3 bucket tại thư mục:

```txt
uploads/
```

Sau khi file được tải lên, S3 có thể kích hoạt một Lambda function thông qua S3 Event Notification để bắt đầu quy trình xử lý hóa đơn.

#### Amazon Textract

Amazon Textract là dịch vụ AI của AWS dùng để trích xuất văn bản, bảng biểu, biểu mẫu và dữ liệu có cấu trúc từ tài liệu như hóa đơn. Trong hệ thống này, Textract được sử dụng để đọc nội dung hóa đơn và giảm thao tác nhập liệu thủ công.

#### OpenAI API

OpenAI API được sử dụng để phân tích và chuẩn hóa phần văn bản được trích xuất bởi Amazon Textract. OpenAI API giúp chuyển đổi dữ liệu OCR thô thành dữ liệu hóa đơn có cấu trúc, ví dụ như tên khách hàng, số hóa đơn, ngày hóa đơn, tổng tiền, loại tiền tệ và thông tin chi tiết của hóa đơn.

{{% notice warning %}}
OpenAI API là một dịch vụ bên ngoài AWS và không nằm trong AWS Cloud. API key cần được lưu trữ an toàn ở phía backend và không được đặt trong mã nguồn React hoặc public repository.
{{% /notice %}}

#### AWS Lambda

AWS Lambda là dịch vụ serverless compute cho phép chạy code backend mà không cần quản lý server. Trong project này, các Lambda functions được sử dụng để xử lý upload hóa đơn, xử lý file sau khi được tải lên S3, gọi Amazon Textract và OpenAI API, cũng như quản lý dữ liệu hóa đơn trong DynamoDB.

Hệ thống sử dụng một số Lambda functions chính như:

- `UploadInvoiceFileFunction`
- `FetchInvoiceDetailsFunction`

#### Amazon DynamoDB

Amazon DynamoDB là cơ sở dữ liệu NoSQL được quản lý hoàn toàn bởi AWS, dùng để lưu dữ liệu hóa đơn sau khi đã được trích xuất và chuẩn hóa. Trong hệ thống này, DynamoDB lưu các thông tin như mã hóa đơn, tên khách hàng, số hóa đơn, ngày hóa đơn, tổng tiền, loại tiền tệ, tags, trạng thái đánh dấu quan trọng và dữ liệu hóa đơn đã được trích xuất.

#### Amazon API Gateway

Amazon API Gateway được sử dụng để tạo và quản lý các REST API endpoint cho ứng dụng. Trong hệ thống này, API Gateway cung cấp các endpoint để upload file hóa đơn, lấy danh sách hóa đơn, xem chi tiết hóa đơn, tìm kiếm hóa đơn và cập nhật metadata như tags hoặc trạng thái starred.

Một số API route trong project gồm:

```txt
POST /uploads
GET /invoice
GET /invoice/{id}
GET /invoice?name=<customer_name>
PATCH /invoice/tags/{id}
PATCH /invoice/starred/{id}
```

{{% notice info %}}
API Gateway chỉ được bảo vệ bằng Amazon Cognito nếu bạn có cấu hình Cognito Authorizer. Nếu API route chưa được gắn Cognito Authorizer, Cognito chỉ đang bảo vệ phần đăng nhập và đăng ký ở frontend.
{{% /notice %}}

#### Amazon Cognito

Amazon Cognito được sử dụng để cung cấp chức năng xác thực người dùng cho ứng dụng frontend. Dịch vụ này cho phép người dùng đăng ký, đăng nhập và truy cập ứng dụng React một cách an toàn. Frontend sử dụng các thông tin cấu hình của Cognito User Pool như User Pool ID, App Client ID và AWS Region.

#### AWS Amplify Hosting

AWS Amplify Hosting được sử dụng để build và deploy ứng dụng React frontend. Frontend được kết nối với GitHub repository và sử dụng environment variables để kết nối đến Cognito và các API Gateway endpoints.

Trong project này, Amplify chỉ được sử dụng cho mục đích hosting frontend. Cognito và các tài nguyên backend được cấu hình riêng thông qua AWS Console.

#### Amazon CloudWatch

Amazon CloudWatch là dịch vụ theo dõi và ghi log của AWS. Trong hệ thống này, CloudWatch được sử dụng để thu thập log từ Lambda functions và API Gateway. Dịch vụ này giúp kiểm tra lỗi upload file, lỗi xử lý hóa đơn, lỗi Textract, lỗi OpenAI API, lỗi DynamoDB và lỗi CORS.

---

### Nội dung

1. [Giới thiệu](1-Introduce/)
2. [Thiết lập môi trường](2-Environmentsetup/)
3. [Xử lý hóa đơn bằng AI](3-AIpoweredinvoiceprocessing/)
4. [Triển khai API Gateway](4-Deployingapigateway/)
5. [Kiểm thử với Postman](5-Testwithpostman/)
6. [Triển khai Frontend](6-Deployingfrontend/)
7. [Dọn dẹp tài nguyên](7-Cleanup/)
