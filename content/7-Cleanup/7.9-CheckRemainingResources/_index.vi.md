+++
title = "Kiểm tra tài nguyên còn sót"
date = 2025
weight = 9
chapter = false
pre = "<b>7.9 </b>"
+++

#### Tổng quan

Trong bước cleanup cuối cùng này, bạn sẽ kiểm tra lại các tài nguyên tùy chọn hoặc tài nguyên phụ có thể vẫn còn tồn tại sau khi đã xóa các thành phần chính của hệ thống **Serverless AI Invoice Scanner**.

Các bước cleanup trước đó đã xóa các tài nguyên chính:

- API Gateway
- Lambda Functions
- S3 Bucket
- DynamoDB Table
- Cognito User Pool
- Amplify Hosting App
- CloudWatch Log Groups
- IAM Roles và Policies

{{% notice info %}}
Mục 7.9 được dùng để kiểm tra các tài nguyên tùy chọn có thể đã được tạo ra tùy theo cách bạn triển khai hoặc kiểm thử dự án.
{{% /notice %}}

---

#### Các tài nguyên cần kiểm tra

Sau khi xóa các tài nguyên chính, chỉ kiểm tra các tài nguyên tùy chọn dưới đây nếu bạn thực sự có sử dụng:

| Tài nguyên | Khi nào cần kiểm tra |
|---|---|
| AWS Secrets Manager | Nếu bạn lưu OpenAI API Key hoặc secret khác trong AWS. |
| Systems Manager Parameter Store | Nếu bạn lưu cấu hình hoặc API key trong Parameter Store. |
| CloudFormation Stack | Nếu bạn triển khai bằng template, SAM hoặc CloudFormation stack. |
| CloudWatch Alarms | Nếu bạn tạo alarm cho Lambda, API Gateway hoặc billing. |
| Resource Groups Tag Editor | Nếu bạn dùng tag để quản lý tài nguyên của dự án. |
| Billing / Cost Explorer | Để xác nhận không còn chi phí AWS phát sinh ngoài dự kiến. |

---

#### Kiểm tra AWS Secrets Manager

Nếu bạn đã lưu **OpenAI API Key** trong AWS Secrets Manager, hãy kiểm tra và xóa secret nếu không còn sử dụng.

- Mở **AWS Management Console**.
- Tìm kiếm và truy cập **Secrets Manager**.
- Tìm các secret liên quan đến dự án.

Ví dụ:

```txt
openai-api-key
invoice-scanner-openai-secret
```

- Mở secret cần kiểm tra.
- Chọn **Actions**.
- Chọn **Delete secret**.
- Chọn thời gian chờ xóa theo yêu cầu của AWS.
- Xác nhận xóa secret.

{{% notice warning %}}
Không xóa secret nếu secret đó vẫn đang được dùng cho dự án khác. OpenAI API Key là thông tin nhạy cảm, không nên lưu trong mã nguồn frontend hoặc public repository.
{{% /notice %}}

---

#### Kiểm tra Systems Manager Parameter Store

Nếu bạn lưu API key hoặc cấu hình trong **AWS Systems Manager Parameter Store**, hãy kiểm tra và xóa các parameter không còn cần thiết.

- Mở **AWS Management Console**.
- Tìm kiếm và truy cập **Systems Manager**.
- Trong thanh điều hướng bên trái, chọn **Parameter Store**.
- Tìm các parameter liên quan đến dự án.

Ví dụ:

```txt
/invoice-scanner/openai-api-key
/invoice-scanner/api-url
/invoice-scanner/s3-bucket
```

- Chọn parameter không còn sử dụng.
- Chọn **Delete**.

{{% notice info %}}
Nếu dự án không sử dụng Parameter Store, bạn có thể bỏ qua phần này.
{{% /notice %}}

---

#### Kiểm tra CloudFormation Stack

Nếu bạn từng triển khai tài nguyên bằng **AWS CloudFormation**, **AWS SAM** hoặc một template hạ tầng khác, hãy kiểm tra xem stack còn tồn tại hay không.

- Mở **AWS Management Console**.
- Tìm kiếm và truy cập **CloudFormation**.
- Chọn **Stacks**.
- Tìm stack liên quan đến dự án.

Ví dụ:

```txt
serverless-ai-invoice-scanner
invoice-scanner-stack
```

- Nếu stack không còn cần thiết, chọn stack và chọn **Delete**.

{{% notice warning %}}
Chỉ xóa CloudFormation stack nếu bạn chắc chắn stack đó được tạo cho bài lab này. Xóa stack có thể xóa nhiều tài nguyên cùng lúc.
{{% /notice %}}

---

#### Kiểm tra CloudWatch Alarms

Nếu bạn đã tạo **CloudWatch Alarms** để theo dõi Lambda, API Gateway hoặc billing, hãy xóa các alarm không còn sử dụng.

- Mở dịch vụ **CloudWatch**.
- Chọn **Alarms**.
- Chọn **All alarms**.
- Tìm alarm liên quan đến dự án.

Ví dụ:

```txt
InvoiceScannerLambdaErrorAlarm
InvoiceScannerAPIGateway5xxAlarm
```

- Chọn alarm không còn sử dụng.
- Chọn **Actions**.
- Chọn **Delete**.

---

#### Kiểm tra tài nguyên bằng Tag Editor

Nếu bạn đã gắn tag cho các tài nguyên của dự án, bạn có thể dùng **Tag Editor** để tìm nhanh các tài nguyên còn sót.

Ví dụ tag:

```txt
Project = ServerlessAIInvoiceScanner
```

Cách sử dụng Tag Editor:

- Mở **AWS Resource Groups & Tag Editor**.
- Chọn **Tag Editor**.
- Chọn region bạn đã triển khai tài nguyên.
- Tìm theo tag hoặc theo loại tài nguyên.
- Kiểm tra xem còn tài nguyên nào liên quan đến dự án hay không.

{{% notice info %}}
Tag Editor sẽ hữu ích nhất nếu bạn đã gắn tag trong quá trình triển khai. Nếu không dùng tag, hãy kiểm tra thủ công các dịch vụ AWS chính.
{{% /notice %}}

---

#### Kiểm tra Billing và Cost Explorer

Sau khi cleanup, bạn nên kiểm tra lại phần chi phí để xác nhận không còn dịch vụ nào phát sinh phí ngoài dự kiến.

- Mở **AWS Billing and Cost Management**.
- Chọn **Bills** hoặc **Cost Explorer**.
- Kiểm tra các dịch vụ đang phát sinh chi phí.
- Chú ý các dịch vụ như:

```txt
Amazon S3
AWS Lambda
Amazon DynamoDB
Amazon API Gateway
Amazon CloudWatch
Amazon Cognito
AWS Secrets Manager
AWS Systems Manager
```

{{% notice warning %}}
Một số dữ liệu billing có thể cập nhật chậm sau vài giờ hoặc vài ngày. Nếu bạn vừa xóa tài nguyên, hãy kiểm tra lại sau một khoảng thời gian.
{{% /notice %}}

---

#### Checklist kiểm tra cuối cùng

Bạn có thể dùng checklist sau để xác nhận đã dọn dẹp xong tài nguyên:

| Mục kiểm tra | Trạng thái |
|---|---|
| API Gateway đã xóa | ☐ |
| Lambda Functions đã xóa | ☐ |
| S3 Bucket đã empty và xóa | ☐ |
| DynamoDB Table đã xóa | ☐ |
| Cognito User Pool đã xóa | ☐ |
| Amplify Hosting App đã xóa | ☐ |
| CloudWatch Log Groups đã xóa | ☐ |
| IAM Roles và Policies không còn dùng đã xóa | ☐ |
| Secrets Manager đã kiểm tra | ☐ |
| Parameter Store đã kiểm tra | ☐ |
| CloudFormation Stack đã kiểm tra | ☐ |
| CloudWatch Alarms đã kiểm tra | ☐ |
| Tag Editor đã kiểm tra nếu có sử dụng tag | ☐ |
| Billing / Cost Explorer đã kiểm tra | ☐ |

---

#### Ảnh hưởng sau khi hoàn tất cleanup

Sau khi hoàn tất toàn bộ quá trình cleanup, hệ thống **Serverless AI Invoice Scanner** sẽ không còn hoạt động trên AWS.

| Thành phần | Trạng thái sau cleanup |
|---|---|
| Frontend | Không còn website deploy trên Amplify. |
| Authentication | Cognito User Pool đã bị xóa. |
| API | API Gateway đã bị xóa. |
| Backend | Lambda Functions đã bị xóa. |
| File storage | S3 Bucket đã bị xóa. |
| Database | DynamoDB Table đã bị xóa. |
| Logs | CloudWatch Logs đã bị xóa. |
| Permissions | IAM Roles và Policies không còn dùng đã được xóa. |
| Optional resources | Secrets Manager, Parameter Store, CloudFormation, CloudWatch Alarms, Tag Editor và Billing đã được kiểm tra nếu có sử dụng. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Không phải mọi lần triển khai đều sử dụng đầy đủ các tài nguyên tùy chọn trong mục 7.9. Bạn chỉ cần xóa những tài nguyên thực sự đã được tạo trong bài lab này.
{{% /notice %}}

{{% notice warning %}}
Không xóa tài nguyên nếu bạn không chắc chắn tài nguyên đó thuộc dự án này. Việc xóa nhầm tài nguyên có thể ảnh hưởng đến các ứng dụng hoặc bài lab khác trong cùng tài khoản AWS.
{{% /notice %}}

{{% notice info %}}
Để tránh phát sinh chi phí trong tương lai, hãy kiểm tra lại AWS Billing sau khi hoàn tất cleanup.
{{% /notice %}}

---

#### Kết luận

Bạn đã hoàn tất bước kiểm tra cuối cùng sau khi dọn dẹp tài nguyên của hệ thống **Serverless AI Invoice Scanner**.

- Các tài nguyên chính đã được xóa.
- Các tài nguyên tùy chọn như Secrets Manager, Parameter Store, CloudFormation và CloudWatch Alarms đã được kiểm tra nếu có sử dụng.
- Tag Editor có thể được dùng để rà soát tài nguyên còn sót nếu dự án có gắn tag.
- Billing đã được rà soát để phát hiện chi phí bất thường.
- Bước cuối cùng này giúp tài khoản AWS gọn gàng hơn và giảm nguy cơ phát sinh chi phí ngoài ý muốn.
