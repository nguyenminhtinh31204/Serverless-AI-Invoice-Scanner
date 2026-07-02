+++
title = "Dọn dẹp tài nguyên"
date = 2025
weight = 7
chapter = false
pre = "<b>7. </b>"
+++

#### Tổng quan

Sau khi hoàn thành việc triển khai và kiểm thử hệ thống **Serverless AI Invoice Scanner**, bạn cần xóa các tài nguyên AWS đã tạo trong quá trình thực hiện lab.

Việc dọn dẹp tài nguyên giúp:

- Tránh phát sinh chi phí không cần thiết.
- Giải phóng các dịch vụ AWS không còn sử dụng.
- Giữ tài khoản AWS gọn gàng và dễ quản lý.
- Đảm bảo các tài nguyên thử nghiệm không tiếp tục hoạt động sau khi kết thúc lab.

Trong phần này, bạn sẽ thực hiện xóa các tài nguyên chính đã được tạo trong hệ thống, bao gồm:

- Amazon API Gateway.
- AWS Lambda Functions.
- Amazon S3 Bucket.
- Amazon DynamoDB Table.
- Amazon Cognito User Pool.
- AWS Amplify Hosting App.
- Amazon CloudWatch Log Groups.
- IAM Roles và Policies.
- Một số tài nguyên tùy chọn khác như AWS Secrets Manager, Parameter Store, CloudFormation hoặc CloudWatch Alarms nếu có sử dụng.

{{% notice warning %}}
Sau khi xóa tài nguyên, dữ liệu liên quan có thể không khôi phục được. Hãy đảm bảo bạn đã sao lưu dữ liệu cần thiết trước khi thực hiện thao tác xóa.
{{% /notice %}}

#### Thứ tự dọn dẹp đề xuất

Nên xóa tài nguyên theo thứ tự sau để tránh lỗi phụ thuộc giữa các dịch vụ:

1. [Xóa API Gateway](7.1-RemoveAPIGateway/)
2. [Xóa Lambda Functions](7.2-RemoveLambdaFunction/)
3. [Xóa S3 Bucket](7.3-RemoveS3Bucket/)
4. [Xóa DynamoDB Table](7.4-RemoveDynamoDB/)
5. [Xóa Cognito User Pool](7.5-RemoveCognitoUserPool/)
6. [Xóa Amplify Hosting App](7.6-RemoveAmplifyHosting/)
7. [Xóa CloudWatch Log Groups](7.7-RemoveCloudWatch/)
8. [Xóa IAM Roles và Policies](7.8-RemoveIAM/)
9. [Xóa các tài nguyên tùy chọn khác nếu có](7.9-CheckRemainingResources/)

{{% notice info %}}
Một số dịch vụ như Amazon Textract không cần xóa tài nguyên riêng vì trong dự án này Textract được gọi trực tiếp từ Lambda và không tạo tài nguyên cố định cần quản lý.
{{% /notice %}}
