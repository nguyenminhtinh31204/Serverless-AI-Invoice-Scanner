---
title: "Test Lambda Function #2"
weight: 5
chapter: false
pre: " <b> 3.5 </b> "
---

#### Overview

In this step, you will test the **FetchInvoiceDetailsFunction** Lambda Function. This function is responsible for reading and updating invoice information from DynamoDB through API endpoints such as GET or PATCH. Testing will help verify that the Lambda function works correctly when receiving input from API Gateway.

{{% notice warning %}}
⚠️ Make sure you already have at least one invoice file in your S3 Bucket and a corresponding record in the **InvoiceData** DynamoDB table before starting the test.
{{% /notice %}}

---

#### Step 1: Create a Test Event for Data Retrieval

1. Open the **AWS Lambda Console**.

    ![Open Lambda Console](/images/3.lambdafunctions/3.5-testfetch/001-openlambda.png)

2. Select the **FetchInvoiceDetailsFunction** function.

    ![Lambda Function](/images/3.lambdafunctions/3.5-testfetch/002-selectfunction.png)

3. Switch to the **Test** tab.

4. Scroll down to the **Test event** section and configure it as follows:

    - **Event name**: `TestGetInvoice`
    - **Template**: Hello World

    ![Test event](/images/3.lambdafunctions/3.5-testfetch/004-createevent.png)

5. Paste the following JSON into the event content:

    ```json
    {
        "httpMethod": "GET",
        "path": "/invoice/demo_invoice.png",
        "pathParameters": {
            "id": "Your_InvoiceId"
        }
    }
    ```

> 📌 Replace the `"id"` value with a valid **InvoiceId** that exists in the **InvoiceData** DynamoDB table.

![JSON](/images/3.lambdafunctions/3.5-testfetch/005-pastejson.png)

6. Scroll up and click **Save**.

    ![Save](/images/3.lambdafunctions/3.5-testfetch/006-saveevent.png)

---

#### Step 2: Run the Test

1. After creating the Test Event, click the **Test** button to execute it.

    ![Test](/images/3.lambdafunctions/3.5-testfetch/007-test.png)

2. Observe the **Execution results** displayed after running:

    - If successful, you will see: **Status: succeeded** along with log output showing the processing details.

    ![Execution results](/images/3.lambdafunctions/3.5-testfetch/008-executionresult.png)
