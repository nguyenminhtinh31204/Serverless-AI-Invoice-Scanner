---
title: "Giới thiệu"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

#### Giới thiệu

Trong bài lab này, bạn sẽ xây dựng **Hệ thống Quét Hóa Đơn AI Không Máy Chủ (Serverless)** sử dụng các dịch vụ AWS. Giải pháp này bao gồm các thành phần chính sau:

-   **Amazon S3** – lưu trữ các tệp hóa đơn được tải lên.
-   **AWS Lambda** – xử lý hóa đơn, kết hợp Amazon Textract và Bedrock.
-   **Amazon DynamoDB** – lưu trữ dữ liệu đã trích xuất và chuẩn hóa.
-   **Amazon API Gateway** – cung cấp các API bảo mật để tải lên và truy vấn dữ liệu hóa đơn.
-   **AWS Amplify** – triển khai và quản lý ứng dụng giao diện người dùng.
-   **Amazon Cognito** – quản lý xác thực người dùng và kiểm soát quyền truy cập API.
-   **Amazon CloudWatch** – giám sát log, chỉ số và tình trạng tổng thể của hệ thống.

#### Kiến thức đạt được

Sau khi hoàn thành bài lab, bạn sẽ có kinh nghiệm thực hành về:

-   Thiết kế và xây dựng kiến trúc AI serverless.
-   Sử dụng Amazon Textract và Bedrock để trích xuất và làm giàu dữ liệu hóa đơn.
-   Tạo API bảo mật và có khả năng mở rộng với API Gateway, Lambda và DynamoDB.
-   Triển khai giao diện hiện đại với AWS Amplify và bảo mật bằng Cognito.
-   Giám sát hiệu suất, tối ưu chi phí và dọn dẹp tài nguyên AWS.
