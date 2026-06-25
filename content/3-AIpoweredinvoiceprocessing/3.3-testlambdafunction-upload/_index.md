---
title: "Testing Lambda Function #1"
weight: 3
chapter: false
pre: " <b> 3.3 </b> "
---

#### Tổng quan

In this step, you will test the **UploadInvoiceFileFunction** Lambda Function by uploading a sample invoice file to the S3 bucket. The purpose of this test is to verify the entire processing workflow, from file upload to S3, data extraction with Textract, analysis with Bedrock, and storage in DynamoDB.

---

#### Prerequisites

Before testing the Lambda function, prepare a sample invoice file to upload. Please download the following file:

-   [Download demo_invoice.png sample file](/images/demo_invoice.png)

{{% notice info %}}
🔧 **Note**: If you use a different invoice file, rename it to **demo_invoice.png** before uploading to the **uploads/** folder in S3.  
{{% /notice %}}

---

#### Step 1: Upload Invoice File to S3

1. Go to **Amazon S3 Console**.

![Amazon S3 Console](/images/3.lambdafunctions/3.3-testupload/001-s3console.png)

2. Open the bucket named: **invoice-upload-s3-bucket**.

![Access S3 Bucket](/images/3.lambdafunctions/3.3-testupload/002-accesss3bucket.png)

3. Open the **uploads/** folder.

![Open Folder](/images/3.lambdafunctions/3.3-testupload/003-openfolder.png)

4. Click **Upload**.

![Click Upload](/images/3.lambdafunctions/3.3-testupload/004-clickupload.png)

5. Click **Add files**.

![Click Add Files](/images/3.lambdafunctions/3.3-testupload/005-clickaddfiles.png)

6. Choose the file: **demo_invoice.png**.

![Choose File](/images/3.lambdafunctions/3.3-testupload/006-demo-invoice.png)

7. Click **Upload** to upload the file.

![Upload file to S3](/images/3.lambdafunctions/3.3-testupload/007-uploadfiletos3.png)

8. Check the file after upload.

![Check file](/images/3.lambdafunctions/3.3-testupload/008-uploadfilesuccess.png)

---

#### Step 2: Create Test Event in Lambda Console

1. Open **AWS Lambda Console**.

![AWS Lambda Console](/images/3.lambdafunctions/3.3-testupload/009-lambdaconsole.png)

2. Navigate to the **UploadInvoiceFileFunction** function.

3. Click the **Test** tab to create a new Test Event.

4. **Event name**: `TestUploadInvoice`.

![Test event](/images/3.lambdafunctions/3.3-testupload/010-testevent.png)

5. Paste the following JSON content into the event section:

```json
{
    "Records": [
        {
            "eventVersion": "2.1",
            "eventSource": "aws:s3",
            "awsRegion": "us-east-1",
            "eventTime": "2025-07-31T12:00:00.000Z",
            "eventName": "ObjectCreated:Put",
            "s3": {
                "bucket": {
                    "name": "invoice-upload-s3-bucket"
                },
                "object": {
                    "key": "uploads/demo_invoice.png"
                }
            }
        }
    ]
}
```

![Paste JSON](/images/3.lambdafunctions/3.3-testupload/011-json.png)

6. Click **Save**.

![Save](/images/3.lambdafunctions/3.3-testupload/012-save.png)

---

#### Step 3: Run the Test

1.  After creating the Test Event, click the **Test** button to run it.

![Test event](/images/3.lambdafunctions/3.3-testupload/013-test.png)

2.  Observe the **Execution results** displayed after the run:

    -   If successful, you will see: **Status: succeeded** along with log output.

![Execution function](/images/3.lambdafunctions/3.3-testupload/014-executionfunction.png)

#### Step 4: View Detailed Logs in CloudWatch

1.  In Lambda Console, select the **Monitor** tab.

![Tab Monitor](/images/3.lambdafunctions/3.3-testupload/015-tabmonitor.png)

2.  Click **View CloudWatch logs**.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/016-viewcloudwatchlogs.png)

3.  Open the **latest log stream**.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/017-viewcloudwatchlogs.png)

4.  Review the logs.

![View CloudWatch logs](/images/3.lambdafunctions/3.3-testupload/018-checklogs.png)

---

#### Step 5: Check Data in DynamoDB

1.  Go to the **AWS DynamoDB Console**.

![AWS DynamoDB Console](/images/3.lambdafunctions/3.3-testupload/019-dynamodbconsole.png)

2.  Open the **InvoiceData** table.

![InvoiceData](/images/3.lambdafunctions/3.3-testupload/020-invoicedata.png)

3.  Click **Explore table items**.

![Explore table items](/images/3.lambdafunctions/3.3-testupload/021-exploretableitems.png)

4.  Search for the record with **InvoiceId** corresponding to **demo_invoice.pn**g to confirm it was saved.

![Explore table items](/images/3.lambdafunctions/3.3-testupload/022-exploretableitems.png)

{{% notice warning %}}
⚠️ **Warning**: Ensure all resources (Lambda, S3, DynamoDB, Textract, and Bedrock) are in the same region: **N. Virginia (us-east-1)** to ensure proper system synchronization.
{{% /notice %}}
