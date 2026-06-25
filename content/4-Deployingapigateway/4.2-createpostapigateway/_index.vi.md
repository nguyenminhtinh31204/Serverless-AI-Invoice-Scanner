---
title: "Tạo API Gateway (POST)"
weight: 2
chapter: false
pre: " <b> 4.2 </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ tạo **REST API Gateway** để tiếp nhận hóa đơn người dùng gửi lên qua endpoint **POST /uploads**. API này sẽ tích hợp với Lambda Function #1 **UploadInvoiceFileFunction**, tự động xử lý ảnh/tệp hóa đơn bằng AI và lưu kết quả vào DynamoDB.

---

#### Bước 1: Tạo REST API

1. Đăng nhập vào AWS Management Console, tìm và truy cập **API Gateway**.

![Open API Gateway](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/001-openapigateway.png)

2. Nhấn **Create API**.

![Create API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/002-createapi.png)

3. Chọn loại **REST API**, sau đó nhấn **Build**.

![Build REST API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/003-buildrestapi.png)

4. Trong phần cấu hình:

    - **API name**: `PostInvoiceAPI`
    - **Description**: `Post Upload Invoice File by API Gateway`
    - **Endpoint Type**: chọn **Regional**.

5. Nhấn **Create API** để hoàn tất.

![Create API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/004-createapi.png)

---

#### Bước 2: Tạo Resource

##### Resource: /uploads

1.  Trong API **PostInvoiceAPI**, chọn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/005-createresource.png)

2.  Nhập thông tin:

    -   **Resource path**: `/`
    -   **Resource name**: `uploads`

3.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/006-createresource.png)

#### Bước 3: Tạo Method

##### Method: POST

1. Trong cây tài nguyên, chọn resource **`/uploads`**.

2. Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/007-createmethodd.png)

3. Cấu hình trong phần **Create method**:

    - **Method type**: POST.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Bật.
    - **Lambda function**: UploadInvoiceFileFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/008-configuration.png)

4. Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/009-createmethod.png)

##### Method: PUT

1. Trong cây tài nguyên, chọn resource **`/uploads`**.

2. Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/010-createmethod.png)

3. Cấu hình trong phần **Create method**:

    - **Method type**: PUT.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Bật.
    - **Lambda function**: UploadInvoiceFileFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/011-configuration.png)

4. Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/009-createmethod.png)

#### Bước 4: Bật CORS cho các method

1.  Trong cây tài nguyên của API **PostInvoiceAPI**, chọn resource **`/uploads`**.
2.  Nhấn **Enable CORS**.

![Enable CORS](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/012-enable-cors.png)

3.  Tại **Access-Control-Allow-Methods**, bật CORS cho:

    -   POST
    -   PUT

4.  Nhấn **Save**.

![Save CORS](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/013-save-cors.png)

#### Bước 5: Deploy API

1. Nhấn **Deploy API**.

![Deploy API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/014-deploy-api.png)

2. Trong modal **Deploy API**:

    - Stage: **[New Stage]**.
    - Stage name: `dev`.
    - Deployment description: `Test API Method POST`.
    - Nhấn **Deploy**.

![New stage](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/015-newstage.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/016-configuration.png)
