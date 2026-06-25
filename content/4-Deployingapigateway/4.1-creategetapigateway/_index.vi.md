---
title: "Tạo API Gateway (GET)"
weight: 1
chapter: false
pre: " <b> 4.1 </b> "
---

#### Tổng quan

Phần này hướng dẫn bạn tạo REST API tên **GetInvoiceAPI** với các route để truy vấn và cập nhật dữ liệu hóa đơn. Mỗi route được kết nối với Lambda và bật CORS, sau đó deploy với stage dev.

---

#### Bước 1: Tạo REST API

1. Đăng nhập vào AWS Management Console, tìm và truy cập **API Gateway**.

![Open API Gateway](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/001-openapigateway.png)

2. Nhấn **Create an API**.

![Create API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/002-createapi.png)

3. Chọn loại **REST API**, sau đó nhấn **Build**.

![Build REST API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/003-buildrestapi.png)

4. Trong phần cấu hình:

    - **API name**: `GetInvoiceAPI`
    - **Description**: `Post Invoice File by API Gateway`
    - **Endpoint Type**: chọn **Regional**.

![API Details](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/004-apidetails.png)

5. Nhấn **Create API** để hoàn tất.

![Create API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/005-createapi.png)

---

#### Bước 2: Tạo các Resource & Method

##### Resource: /invoice

1.  Trong API **GetInvoiceAPI**, chọn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/006-createresource.png)

2.  Nhập thông tin:

    -   **Resource path**: `/`
    -   **Resource name**: `invoice`

3.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/006-createresource.png)

4.  Sau khi resource **`/invoice`** được tạo, chọn lại nó trong cây tài nguyên.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/007-resourceinvoice.png)

5.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/008-createmethod.png)

6.  Cấu hình trong phần **Create method**:

    -   **Method type**: GET.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Bật.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/009-configuration.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/010-configuration.png)

7.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

8.  API Gateway sẽ được tạo và chuyển đến trang cấu hình chi tiết của API Gateway.

![Configuration details](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/012-configurationdetails.png)

##### Resource: /invoice/{id}

1.  Trong cây tài nguyên, chọn resource **`/invoice`**.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/013-resourceinvoice.png)

2.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/014-createresource.png)

3.  Nhập thông tin:

    -   **Resource path**: `/invoice/`
    -   **Resource name**: `{id}`

4.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/015-createresource.png)

5.  Sau khi resource **`/invoice/{id}`** được tạo, chọn lại nó trong cây tài nguyên.

![Resource /invoice/{id}](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/016-resource-invoice-id.png)

6.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/017-create-method.png)

7.  Cấu hình trong phần **Create method**:

    -   **Method type**: GET.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Bật.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/018-configuration.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/019-configuration.png)

8.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

9.  Tiếp tục thêm method **PATCH**.

    -   Chọn lại nó trong cây tài nguyên và nhấn **Create method**.

![Choose resource & click create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/020-resource-and-createmethod.png)

    -   Thực hiện cấu hình tương tự như trên:

        -   **Method type**: PATCH
        -   **Integration type**: Lambda Function
        -   **Use Lambda Proxy integration**: Bật
        -   **Lambda Function**: FetchInvoiceDetailsFunction

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/021-configuration.png)

10. Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/starred

1.  Trong API **GetInvoiceAPI**, chọn resource **`/invoice`**.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/022-resourceinvoice.png)

2.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/023-createresource.png)

3.  Cấu hình:

    -   **Resource path**: `/invoice/`
    -   **Resource name**: `starred`

4.  Nhấn **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/024-configuration-createresource.png)

5.  Sau khi resource được tạo, chọn lại **`/invoice/starred`** trong cây tài nguyên.

![Resource /invoice/starred](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/025-resource-invoice-starred.png)

6.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/026-createmethod.png)

7.  Cấu hình trong phần **Create method**:

    -   **Method type**: GET.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Bật.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/027-configuration.png)

8.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/starred/{id}

1.  Trong API **GetInvoiceAPI**, chọn resource **`/invoice/starred`**.

![Resource /invoice/starred](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/028-resource-invoice-starred.png)

2.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/029-createresource.png)

3.  Cấu hình:

    -   **Resource path**: `/invoice/starred/`
    -   **Resource name**: `{id}`

4.  Nhấn **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/030-configuration-createresource.png)

5.  Sau khi resource được tạo, chọn lại **`/invoice/starred/{id}`** trong cây tài nguyên.
6.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/031-createmethod.png)

7.  Cấu hình trong phần **Create method**:

    -   **Method type**: PATCH.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Bật.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/032-configuration.png)

8.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/tags

1.  Trong API **GetInvoiceAPI**, chọn resource **`/invoice`**.

![Resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/033-resource-invoice.png)

2.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/034-createresource.png)

3.  Cấu hình:

    -   **Resource path**: `/invoice/`
    -   **Resource name**: `tags`

4.  Nhấn **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/035-configuration-createresource.png)

##### Resource: /invoice/tags/{id}

1.  Trong cây tài nguyên, chọn lại **`/invoice/tags`**.
2.  Nhấn **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/036-createresource.png)

3.  Cấu hình:

    -   **Resource path**: `/invoice/tags/`
    -   **Resource name**: `{id}`

4.  Nhấn **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/037-configuration-createresource.png)

5.  Sau khi resource được tạo, chọn lại **`/invoice/tags/{id}`** trong cây tài nguyên.
6.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/038-createmethod.png)

7.  Cấu hình trong phần **Create method**:

    -   **Method type**: PATCH.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Bật.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/032-configuration.png)

8.  Nhấn **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

#### Bước 3: Bật CORS cho các method

1.  Trong cây tài nguyên của API **GetInvoiceAPI**, chọn resource **`/invoice/{id}`**.
2.  Nhấn **Enable CORS**.

![Enable CORS](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/039-enable-cors.png)

3.  Tại **Access-Control-Allow-Methods**, bật CORS cho:

    -   GET
    -   PATCH

4.  Nhấn **Save**.

![Save CORS](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/040-save-cors.png)

5.  Lặp lại các bước trên cho từng resource còn lại:

    -   **/invoice/starred/{id}**: bật CORS cho `PATCH`
    -   **/invoice/tags/{id}**: bật CORS cho `PATCH`

![Result](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/041-result-cors.png)

![Result](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/042-result-cors.png)

#### Bước 4: Deploy API

1. Nhấn **Deploy API**.

![Deploy API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/043-deploy-api.png)

2. Trong modal **Deploy API**:

    - Stage: **[New Stage]**.
    - Stage name: `dev`.
    - Deployment description: `Test API Method GET`.
    - Nhấn **Deploy**.

![New stage](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/044-newstage.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/045-configuration.png)
