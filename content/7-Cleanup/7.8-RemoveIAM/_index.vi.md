+++
title = "Xóa IAM Roles và Policies"
date = 2025
weight = 8
chapter = false
pre = "<b>7.8 </b>"
+++

#### Tổng quan

Trong bước này, bạn sẽ xóa các **IAM Roles** và **IAM Policies** đã được tạo cho hệ thống **Serverless AI Invoice Scanner**.

AWS Identity and Access Management (IAM) được sử dụng để cấp quyền cho các dịch vụ trong hệ thống. Ví dụ, Lambda cần quyền để ghi file vào Amazon S3, đọc file từ S3, gọi Amazon Textract, ghi dữ liệu vào DynamoDB và ghi log vào CloudWatch.

Sau khi hoàn thành bài lab và đã xóa các tài nguyên chính như API Gateway, Lambda, S3, DynamoDB, Cognito và Amplify Hosting, bạn nên kiểm tra và xóa các IAM Roles hoặc Policies không còn sử dụng.

{{% notice warning %}}
Không nên xóa IAM Role hoặc IAM Policy nếu bạn chưa chắc chắn tài nguyên đó không còn được sử dụng bởi dịch vụ khác. Xóa nhầm IAM Role có thể làm các Lambda Function hoặc dịch vụ AWS khác bị lỗi quyền truy cập.
{{% /notice %}}

---

#### Tài nguyên cần kiểm tra

Trong dự án này, các IAM Roles thường được tạo hoặc sử dụng cho các Lambda Functions.

Các role có thể có tên tương tự:

```txt
UploadInvoiceFileFunction-role
FetchInvoiceDetailsFunction-role
```

hoặc các tên tự động do AWS tạo, ví dụ:

```txt
UploadInvoiceFileFunction-role-xxxxxxxx
FetchInvoiceDetailsFunction-role-xxxxxxxx
```

Các IAM Policies liên quan có thể cấp quyền cho:

| Dịch vụ | Quyền sử dụng |
|---|---|
| Amazon S3 | Upload, đọc hoặc lấy file hóa đơn từ bucket. |
| Amazon DynamoDB | Ghi, đọc, cập nhật và truy vấn dữ liệu hóa đơn. |
| Amazon Textract | Gọi dịch vụ OCR để trích xuất văn bản từ hóa đơn. |
| Amazon CloudWatch Logs | Ghi log khi Lambda chạy. |
| AWS Lambda | Cho phép Lambda thực thi với role tương ứng. |

{{% notice info %}}
Tên IAM Role và Policy thực tế có thể khác tùy theo cách bạn tạo Lambda Function hoặc cấu hình quyền trong AWS Console.
{{% /notice %}}

---

#### Kiểm tra IAM Role đang gắn với Lambda

Trước khi xóa IAM Role, bạn nên xác định role nào đã được dùng cho các Lambda Functions của dự án.

- Mở **AWS Management Console**.

- Truy cập dịch vụ **Lambda**.

![Remove IAM Roles](/images/7/7.8/Screenshot_1.png)

- Mở Lambda Function của dự án, ví dụ:

```txt
UploadInvoiceFileFunction
```

- Chọn tab **Configuration**.

- Chọn **Permissions**.

![Remove IAM Roles](/images/7/7.8/Screenshot_2.png)

- Ghi lại tên **Execution role** của Lambda.

Ví dụ:

```txt
LambdaExecutionRole-AIInvoiceScanner
```
![Remove IAM Roles](/images/7/7.8/Screenshot_3.png)

- Lặp lại thao tác này với các Lambda Functions còn lại nếu chúng chưa bị xóa.

---

#### Các bước xóa IAM Role

- Mở **AWS Management Console**.

- Tìm kiếm và truy cập dịch vụ **IAM**.

![Remove IAM Roles](/images/7/7.8/Screenshot_4.png)

- Trong thanh điều hướng bên trái, chọn **Roles**.

![Remove IAM Roles](/images/7/7.8/Screenshot_5.png)

- Trong ô tìm kiếm, nhập tên role liên quan đến dự án.

Ví dụ:

```txt
LambdaExecutionRole-AIInvoiceScanner
```


![Remove IAM Roles](/images/7/7.8/Screenshot_6.png)

- Chọn IAM Role cần xóa.

Ví dụ:

```txt
LambdaExecutionRole-AIInvoiceScanner
```

![Remove IAM Roles](/images/7/7.8/Screenshot_7.png)

- Kiểm tra lại các quyền đang gắn với role trong phần **Permissions**.

- Đảm bảo role này chỉ được sử dụng cho tài nguyên của dự án **Serverless AI Invoice Scanner**.

- Chọn **Delete**.

![Remove IAM Roles](/images/7/7.8/Screenshot_8.png)

- Hộp thoại xác nhận sẽ xuất hiện.

- Nhập nội dung xác nhận nếu AWS Console yêu cầu.

- Chọn **Delete** để xóa IAM Role.

![Remove IAM Roles](/images/7/7.8/Screenshot_9.png)

---

#### Xóa các IAM Roles còn lại

Lặp lại các bước tương tự để xóa các IAM Roles còn lại của dự án.

Các role cần kiểm tra có thể bao gồm:

```txt
UploadInvoiceFileFunction-role
FetchInvoiceDetailsFunction-role
```

Nếu bạn đã tạo role riêng cho API Gateway hoặc các service khác, hãy kiểm tra và xóa nếu chúng không còn được sử dụng.

---

#### Xóa Customer Managed Policies

Nếu trong quá trình triển khai bạn đã tạo **Customer managed policy** riêng cho dự án, bạn cũng nên xóa các policy này sau khi không còn sử dụng.

Ví dụ policy có thể có tên:

```txt
InvoiceScannerS3Policy
InvoiceScannerDynamoDBPolicy
InvoiceScannerTextractPolicy
InvoiceScannerLambdaPolicy
```

Các bước thực hiện:

- Trong dịch vụ **IAM**, chọn **Policies** ở thanh điều hướng bên trái.

![Remove IAM Policies](/images/7/7.8/Screenshot_10.png)

- Tìm policy liên quan đến dự án bằng từ khóa:

```txt
Invoice
```


![Remove IAM Policies](/images/7/7.8/Screenshot_11.png)

- Chọn policy cần xóa.

- Kiểm tra tab **Entities attached** để đảm bảo policy không còn được gắn với role, user hoặc group nào.

![Remove IAM Policies](/images/7/7.8/Screenshot_12.png)

- Nếu policy vẫn còn được gắn với role, hãy detach policy trước.

- Sau đó chọn **Delete**.

![Remove IAM Policies](/images/7/7.8/Screenshot_13.png)

- Xác nhận xóa policy.

---

#### Không xóa AWS Managed Policies

Một số policy là **AWS managed policy**, ví dụ:

```txt
AWSLambdaBasicExecutionRole
AmazonS3FullAccess
AmazonDynamoDBFullAccess
AmazonTextractFullAccess
```

Các policy này do AWS quản lý và có thể được nhiều dịch vụ khác sử dụng.

{{% notice warning %}}
Không nên cố xóa AWS managed policies. Bạn chỉ cần detach chúng khỏi IAM Role của dự án hoặc xóa IAM Role nếu role đó không còn sử dụng.
{{% /notice %}}

---

#### Kiểm tra IAM User dùng để triển khai

Trong quá trình triển khai, bạn có thể đã tạo IAM User để cấu hình AWS CLI hoặc Amplify Hosting.

Ví dụ:

```txt
ai-invoice-scanner-user
```

Nếu IAM User này chỉ dùng cho bài lab và không còn cần thiết, bạn có thể xóa user hoặc xóa access key của user đó.

Các bước kiểm tra:

- Trong dịch vụ **IAM**, chọn **Users**.

- Tìm user đã dùng trong quá trình triển khai.

![Remove IAM User](/images/7/7.8/Screenshot_14.png)

- Mở user và kiểm tra tab **Security credentials**.

- Nếu user không còn sử dụng, bạn có thể xóa **Access keys** trước.

![Remove IAM User](/images/7/7.8/Screenshot_15.png)

- Sau đó xóa IAM User nếu chắc chắn không còn cần dùng.

{{% notice warning %}}
Chỉ xóa IAM User nếu bạn chắc chắn user đó chỉ được tạo cho bài lab. Không xóa user admin hoặc user đang được sử dụng cho các dự án khác.
{{% /notice %}}

---

#### Kiểm tra sau khi xóa

Sau khi xóa IAM Roles và Policies:

- Quay lại danh sách **Roles** trong IAM.
- Tìm lại các role liên quan đến dự án.
- Đảm bảo các role như `UploadInvoiceFileFunction-role`, `ProcessInvoiceFunction-role` và `InvoiceManagementFunction-role` không còn xuất hiện.
- Kiểm tra danh sách **Policies** nếu bạn có tạo customer managed policies.
- Kiểm tra danh sách **Users** nếu bạn có tạo IAM User riêng cho bài lab.

---

#### Ảnh hưởng đến hệ thống

Sau khi xóa IAM Roles và Policies, các dịch vụ còn lại sẽ không thể sử dụng các quyền đã cấp trước đó.

| Thành phần | Ảnh hưởng |
|---|---|
| Lambda Functions | Không thể chạy đúng nếu execution role đã bị xóa. |
| Amazon S3 | Không bị xóa, nhưng Lambda không còn quyền truy cập nếu role bị xóa. |
| Amazon DynamoDB | Không bị xóa, nhưng Lambda không còn quyền đọc/ghi nếu role bị xóa. |
| Amazon Textract | Không bị xóa, nhưng Lambda không còn quyền gọi Textract nếu role bị xóa. |
| CloudWatch Logs | Log cũ vẫn còn nếu chưa xóa, nhưng Lambda không còn quyền ghi log nếu role bị xóa. |
| API Gateway | Không bị ảnh hưởng trực tiếp, nhưng backend Lambda có thể lỗi nếu thiếu role. |
| Amplify Hosting | Không bị ảnh hưởng trực tiếp. |
| Cognito | Không bị ảnh hưởng trực tiếp. |

---

#### Lưu ý quan trọng

{{% notice info %}}
Nên xóa IAM Roles sau khi đã xóa Lambda Functions, vì các Lambda Functions đang hoạt động cần execution role để chạy.
{{% /notice %}}

{{% notice warning %}}
Không xóa các IAM Roles hoặc Policies có liên quan đến dịch vụ khác ngoài bài lab. Hãy kiểm tra kỹ tên, quyền và phần tài nguyên đang sử dụng trước khi xóa.
{{% /notice %}}

{{% notice info %}}
Nếu bạn lưu OpenAI API Key trong AWS Secrets Manager, IAM Role có thể có quyền đọc secret. Sau khi xóa Lambda và IAM Role, bạn nên kiểm tra thêm Secrets Manager nếu muốn dọn dẹp hoàn toàn.
{{% /notice %}}

---

#### Kết luận

Bạn đã hoàn thành bước kiểm tra và xóa các IAM Roles, IAM Policies hoặc IAM User không còn sử dụng trong hệ thống **Serverless AI Invoice Scanner**.

- Các Lambda execution roles của dự án đã được xóa.
- Các customer managed policies không còn sử dụng đã được xóa.
- IAM User hoặc access key dùng riêng cho bài lab có thể được xóa nếu không còn cần thiết.
- Các quyền truy cập không còn sử dụng đã được loại bỏ khỏi tài khoản AWS.
- Bước dọn dẹp này giúp giảm rủi ro bảo mật và giữ tài khoản AWS gọn gàng hơn.
