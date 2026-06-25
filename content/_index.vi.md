---
title: "Xây dựng hệ thống quét hóa đơn AI trên kiến trúc Serverless"
weight: 1
chapter: false
---

# Xây dựng hệ thống quét hóa đơn AI trên kiến trúc Serverless

### Tổng quan

Trong bài lab này, bạn sẽ xây dựng một hệ thống quét hóa đơn AI theo kiến trúc Serverless trên AWS. Ứng dụng này cho phép người dùng upload hóa đơn, sau đó trích xuất dữ liệu bằng Amazon Textract, phân tích và chuẩn hóa dữ liệu bằng Amazon Bedrock, lưu trữ trong Amazon DynamoDB và truy xuất thông qua API Gateway.

Frontend được triển khai trên AWS Amplify, hỗ trợ đăng nhập và phân quyền với Amazon Cognito. Hệ thống sử dụng CloudWatch để giám sát.

![ConnectPrivate](/images/arc-logg.png)

#### Amazon S3

Amazon S3 (Simple Storage Service) là dịch vụ lưu trữ đối tượng (object storage) của AWS, được sử dụng để lưu trữ file hóa đơn và các tài liệu liên quan. Nó đảm bảo độ bền dữ liệu cao, khả năng mở rộng linh hoạt và truy cập nhanh chóng từ bất kỳ đâu.

#### Amazon Textract

Amazon Textract là dịch vụ AI dùng để trích xuất văn bản, bảng biểu và dữ liệu có cấu trúc từ các hóa đơn. Textract giúp loại bỏ việc nhập liệu thủ công, tăng tốc quá trình số hóa tài liệu.

#### Amazon Bedrock

Amazon Bedrock là dịch vụ cung cấp mô hình AI dưới dạng serverless, dùng để phân tích, hiểu và chuẩn hóa dữ liệu hóa đơn. Nó cho phép xử lý thông tin nâng cao mà không cần quản lý hạ tầng AI.

#### AWS Lambda

AWS Lambda là dịch vụ điện toán không máy chủ (serverless) cho phép chạy code để xử lý các sự kiện như upload hóa đơn, trích xuất và lưu trữ dữ liệu, mà không cần quản lý máy chủ.

#### Amazon DynamoDB

Amazon DynamoDB là cơ sở dữ liệu NoSQL được quản lý hoàn toàn, dùng để lưu trữ dữ liệu hóa đơn sau khi đã được trích xuất và phân tích. DynamoDB cung cấp hiệu năng nhanh, khả năng mở rộng tự động và độ tin cậy cao.

#### Amazon API Gateway

Amazon API Gateway là dịch vụ tạo, quản lý và bảo mật các API. Trong hệ thống này, nó cung cấp các API để upload file hóa đơn và truy xuất dữ liệu đã được lưu trữ.

#### Amazon Cognito

Amazon Cognito là dịch vụ quản lý xác thực và phân quyền người dùng. Nó cung cấp đăng nhập an toàn cho ứng dụng frontend, tích hợp dễ dàng với API Gateway và các dịch vụ AWS khác.

#### AWS Amplify

AWS Amplify là nền tảng phát triển và triển khai frontend, giúp xây dựng giao diện người dùng hiện đại và kết nối trực tiếp với các API backend. Hệ thống frontend của ứng dụng được triển khai qua Amplify.

#### Amazon CloudWatch

Amazon CloudWatch là dịch vụ giám sát hệ thống, thu thập và phân tích log, chỉ số (metrics) và sự kiện từ toàn bộ các thành phần trong hệ thống. Nó giúp theo dõi hiệu suất và cảnh báo sớm các vấn đề.

### Nội dung

1.  [Giới thiệu](1-Introduce/)
2.  [Chuẩn bị môi trường](2-Environmentsetup/)
3.  [Xử lý hóa đơn bằng AI](3-AIpoweredinvoiceprocessing/)
4.  [Triển khai API Gateway](4-Deployingapigateway/)
5.  [Kiểm thử với Postman](5-Testwithpostman/)
6.  [Triển khai Frontend](6-Deployingfrontend/)
7.  [Thực hành End-to-End](7-Endtoendhandson/)
8.  [Dọn dẹp tài nguyên](8-Cleanup/)
