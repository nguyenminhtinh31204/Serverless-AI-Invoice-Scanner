---
title: "Create API Gateway (GET)"
weight: 1
chapter: false
pre: " <b> 4.1 </b> "
---

#### Tổng quan

This section guides you through creating a REST API named **GetInvoiceAPI** with routes for querying and updating invoice data. Each route is connected to a Lambda function and has CORS enabled, then deployed with a `dev` stage.

---

#### Step 1: Create REST API

1. Sign in to the AWS Management Console, search for and open **API Gateway**.

![Open API Gateway](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/001-openapigateway.png)

2. Click **Create an API**.

![Create API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/002-createapi.png)

3. Select **REST API**, then click **Build**.

![Build REST API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/003-buildrestapi.png)

4. Configure the following:

    - **API name**: `GetInvoiceAPI`
    - **Description**: `Post Invoice File by API Gateway`
    - **Endpoint Type**: Select **Regional**.

![API Details](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/004-apidetails.png)

5. Click **Create API** to finish.

![Create API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/005-createapi.png)

---

#### Step 2: Create Resources & Methods

##### Resource: /invoice

1. In **GetInvoiceAPI**, choose **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/006-createresource.png)

2. Enter:

    - **Resource path**: `/`
    - **Resource name**: `invoice`

3. Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/006-createresource.png)

4. After creating **/invoice**, select it in the resource tree.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/007-resourceinvoice.png)

5. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/008-createmethod.png)

6. Configure in **Create method**:

    - **Method type**: GET.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Enabled.
    - **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/009-configuration.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/010-configuration.png)

7. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

8. The API Gateway will be created and redirected to the API Gateway details configuration page.

![Configuration details](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/012-configurationdetails.png)

##### Resource: /invoice/{id}

1. In the resource tree, select the **`/invoice`** resource.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/013-resourceinvoice.png)

2. In the resource tree, select the **`/invoice`** resource.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/014-createresource.png)

3. Enter the following:

    - **Resource path**: `/invoice/`
    - **Resource name**: `{id}`

4. Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/015-createresource.png)

5. After the **`/invoice/{id}`** resource is created, select it again in the resource tree.

![Resource /invoice/{id}](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/016-resource-invoice-id.png)

6. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/017-create-method.png)

7. Configure the **Create method** section:

    - **Method type**: GET.
    - **Integration type**: Lambda function.
    - **Lambda proxy integration**: Enabled.
    - **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/018-configuration.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/019-configuration.png)

8. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

9. Proceed to add a **PATCH** method.

    - Select the resource in the tree and click **Create method** again.

![Choose resource & click create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/020-resource-and-createmethod.png)

    - Configure similarly:

        - **Method type**: PATCH
        - **Integration type**: Lambda Function
        - **Use Lambda Proxy integration**: Enabled
        - **Lambda Function**: FetchInvoiceDetailsFunction

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/021-configuration.png)

10. Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/starred

1.  In the **GetInvoiceAPI**, select the **`/invoice`** resource.

![resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/022-resourceinvoice.png)

2.  Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/023-createresource.png)

3.  Configure:

    -   **Resource path**: `/invoice/`
    -   **Resource name**: `starred`

4.  Click **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/024-configuration-createresource.png)

5.  After the resource is created, select **`/invoice/starred`** in the tree.

![Resource /invoice/starred](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/025-resource-invoice-starred.png)

6.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/026-createmethod.png)

7.  Configure the **Create method**:

    -   **Method type**: GET.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Enabled.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/027-configuration.png)

8.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/starred/{id}

1.  In **GetInvoiceAPI**, select **`/invoice/starred`**.

![Resource /invoice/starred](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/028-resource-invoice-starred.png)

2.  Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/029-createresource.png)

3.  Configure:

    -   **Resource path**: `/invoice/starred/`
    -   **Resource name**: `{id}`

4.  Click **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/030-configuration-createresource.png)

5.  After the resource is created, select **`/invoice/starred/{id}`**.
6.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/031-createmethod.png)

7.  Configure the **Create method**:

    -   **Method type**: PATCH.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Enabled.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/032-configuration.png)

8.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

##### Resource: /invoice/tags

1.  In **GetInvoiceAPI**, select **`/invoice`**.

![Resource /invoice](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/033-resource-invoice.png)

2.  Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/034-createresource.png)

3.  Configure:

    -   **Resource path**: `/invoice/`
    -   **Resource name**: `tags`

4.  Click **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/035-configuration-createresource.png)

##### Resource: /invoice/tags/{id}

1.  In the resource tree, select **`/invoice/tags`**.
2.  Click **Create resource**.

![Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/036-createresource.png)

3.  Configure:

    -   **Resource path**: `/invoice/tags/`
    -   **Resource name**: `{id}`

4.  Click **Create resource**.

![Configuration & Create resource](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/037-configuration-createresource.png)

5.  After the resource is created, select **`/invoice/tags/{id}`**.
6.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/038-createmethod.png)

7.  Configure the **Create method**:

    -   **Method type**: PATCH.
    -   **Integration type**: Lambda function.
    -   **Lambda proxy integration**: Enabled.
    -   **Lambda function**: FetchInvoiceDetailsFunction.

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/032-configuration.png)

8.  Click **Create method**.

![Create method](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/011-createmethod.png)

#### Step 3: Enable CORS for methods

1.  In the resource tree of **GetInvoiceAPI**, select **`/invoice/{id}`**.
2.  Click **Enable CORS**.

![Enable CORS](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/039-enable-cors.png)

1.  In **Access-Control-Allow-Methods**, Enabled CORS:

    -   GET
    -   PATCH

2.  Click **Save**.

![Save CORS](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/040-save-cors.png)

5.  Repeat the above steps for the following resources:

    -   **/invoice/starred/{id}**: Enabled CORS for `PATCH`
    -   **/invoice/tags/{id}**: Enabled CORS for `PATCH`

![Result](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/041-result-cors.png)

![Result](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/042-result-cors.png)

#### Step 4: Deploy API

1. Click **Deploy API**.

![Deploy API](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/043-deploy-api.png)

2. In the **Deploy API** modal:

    - Stage: **[New Stage]**.
    - Stage name: `dev`.
    - Deployment description: `Test API Method GET`.
    - Click **Deploy**.

![New stage](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/044-newstage.png)

![Configuration](/images/4.deployingapigatewayandfrontend/4.1-creategetapigateway/045-configuration.png)
