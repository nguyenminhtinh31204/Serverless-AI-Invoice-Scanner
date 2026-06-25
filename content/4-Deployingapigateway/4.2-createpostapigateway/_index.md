---
title: "Create API Gateway (POST)"
weight: 2
chapter: false
pre: " <b> 4.2 </b> "
---

#### Tổng quan

In this section, you will create a **REST API Gateway** to receive invoices uploaded by users through the **POST /uploads** endpoint. This API will integrate with Lambda Function #1 **UploadInvoiceFileFunction**, automatically process invoice images/files using AI, and store the results into DynamoDB.

---

#### Step 1: Create a REST API

1. Log in to the AWS Management Console, find and access **API Gateway**.

![Open API Gateway](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/001-openapigateway.png)

2. Click **Create API**.

![Create API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/002-createapi.png)

3. Choose **REST API** type, then click **Build**.

![Build REST API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/003-buildrestapi.png)

4. In the configuration section:

    - **API name**: `PostInvoiceAPI`
    - **Description**: `Post Upload Invoice File by API Gateway`
    - **Endpoint Type**: Select **Regional**.

5. Click **Create API** to finish.

![Create API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/004-createapi.png)

---

#### Step 2: Create Resource

##### Resource: /uploads

1. In **PostInvoiceAPI**, select **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/005-createresource.png)

2.  Enter information:

    -   **Resource path**: `/`
    -   **Resource name**: `uploads`

3.  Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/006-createresource.png)

#### Step 3: Create Method

##### Method: POST

1. In the resource tree, select **`/uploads`**.

2. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/007-createmethodd.png)

3. Configure in **Create method**:

    - **Method type**: POST.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Enabled.
    - **Lambda function**: UploadInvoiceFileFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/008-configuration.png)

4. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/009-createmethod.png)

##### Method: PUT

1. In the resource tree, select **`/uploads`**.

2. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/010-createmethod.png)

3. Configure in **Create method**:

    - **Method type**: PUT.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Enabled.
    - **Lambda function**: UploadInvoiceFileFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/011-configuration.png)

4. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/009-createmethod.png)

#### Step 4: Enable CORS for methods

1. In the resource tree of **PostInvoiceAPI**, select resource **`/uploads`**.
2. Click **Enable CORS**.

![Enable CORS](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/012-enable-cors.png)

3.  In **Access-Control-Allow-Methods**, enable CORS for:

    -   POST
    -   PUT

4.  Click **Save**.

![Save CORS](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/013-save-cors.png)

#### Step 5: Deploy the API

1. Click **Deploy API**.

![Deploy API](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/014-deploy-api.png)

2. In the **Deploy API** modal:

    - Stage: **[New Stage]**.
    - Stage name: `dev`.
    - Deployment description: `Test API Method POST`.
    - Click **Deploy**.

![New stage](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/015-newstage.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.2-createpostapigateway/016-configuration.png)
