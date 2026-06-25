---
title: "Tạo IAM Role cho Lambda"
weight: 1
chapter: false
pre: " <b> 3.1 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo một **IAM Role chuyên biệt** để các Lambda function trong hệ thống **Serverless Invoice Scanner** có thể truy cập vào các dịch vụ AWS như **Amazon Textract**, **Amazon Bedrock**, **Amazon DynamoDB**, **Amazon S3** và **CloudWatch Logs**.

---

#### Bước 1: Truy cập IAM Console

1. Mở [AWS Management Console](https://console.aws.amazon.com/) ở chế độ **ẩn danh (Incognito)** để tránh xung đột session nếu bạn đang đăng nhập bằng nhiều tài khoản.

2. Tìm kiếm và chọn **IAM** trong thanh tìm kiếm.

![Open IAM](/images/3.lambdafunctions/3.1-iamrole/001-openiam.png)

3. Trong thanh điều hướng bên trái, chọn **Roles**, sau đó nhấn **Create role**.

![Create Role](/images/3.lambdafunctions/3.1-iamrole/002-createrole.png)

---

#### Bước 2: Cấu hình Role cho Lambda

1. **Trusted entity type**: chọn **AWS service**
2. **Use case**: chọn **Lambda**

![Select Lambda Use Case](/images/3.lambdafunctions/3.1-iamrole/003-selectlambda.png)

3. Nhấn **Next** để tiếp tục.

![Click Next](/images/3.lambdafunctions/3.1-iamrole/003-clicknext.png)

---

#### Bước 3: Gán quyền cho IAM Role

1. Trong bước **Add permissions**, hãy tìm và tick chọn các policy sau:

    - `AmazonS3FullAccess`
    - `AmazonDynamoDBFullAccess`
    - `AmazonTextractFullAccess`
    - `AmazonBedrockFullAccess`
    - `AWSLambdaBasicExecutionRole`

![AmazonS3FullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazons3fullaccess.png)  
![AmazonDynamoDBFullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazondynamodbfullaccess.png)  
![AmazonTextractFullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazontextractfullaccess.png)  
![AWSLambdaBasicExecutionRole](/images/3.lambdafunctions/3.1-iamrole/004-amazonlambdabasicexecutionrole.png)

2. Nhấn **Next** để tiếp tục.

![Click Next](/images/3.lambdafunctions/3.1-iamrole/004-clicknext.png)

---

#### Bước 4: Đặt tên và hoàn tất

1. **Role name**: `LambdaExecutionRole-AIInvoiceScanner`
2. **Description**: `Role for Lambda to access S3, Textract, Bedrock, DynamoDB, and CloudWatch`

![Name Role](/images/3.lambdafunctions/3.1-iamrole/005-namerole.png)

3. Nhấn **Create role** để hoàn tất.

![Create Role](/images/3.lambdafunctions/3.1-iamrole/005-createrole.png)

4. Sau khi tạo xong, trong **IAM Console**, vào mục **Roles**. Bạn sẽ thấy role `LambdaExecutionRole-AIInvoiceScanner` xuất hiện trong danh sách.

![Check Role Details](/images/3.lambdafunctions/3.1-iamrole/006-checkrole.png)
