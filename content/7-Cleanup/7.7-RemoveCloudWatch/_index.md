+++
title = "Remove CloudWatch Log Groups"
date = 2025
weight = 7
chapter = false
pre = "<b>7.7 </b>"
+++

#### Overview

In this step, you will remove the **Amazon CloudWatch Log Groups** that were created during the deployment and testing of the **Serverless AI Invoice Scanner** system.

Amazon CloudWatch was used to store logs from services such as AWS Lambda and Amazon API Gateway. During testing, these logs helped monitor errors, inspect requests, review invoice processing results, and troubleshoot issues such as CORS errors, Lambda failures, DynamoDB errors, and OpenAI API integration errors.

After completing the lab, you should delete unused CloudWatch Log Groups to avoid keeping unnecessary log data and to reduce potential storage costs.

{{% notice warning %}}
After a CloudWatch Log Group is deleted, all log streams and log events inside it are permanently removed. Make sure you no longer need the logs for debugging or documentation before deleting them.
{{% /notice %}}

---

#### Resources to Remove

In this project, CloudWatch Log Groups were usually created automatically for Lambda functions and API Gateway.

The Lambda Log Groups may have names similar to:

```txt
/aws/lambda/UploadInvoiceFileFunction
/aws/lambda/ProcessInvoiceFunction
/aws/lambda/InvoiceManagementFunction
```

If logging was enabled for API Gateway, you may also see Log Groups related to API Gateway.

For example:

```txt
API-Gateway-Execution-Logs_<api-id>/dev
```

or Log Groups related to:

```txt
PostInvoiceAPI
GetInvoiceAPI
```

{{% notice info %}}
The actual Log Group names may be different depending on your Lambda function names, API Gateway names, and deployment stage.
{{% /notice %}}

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **CloudWatch** service.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_1.png)

- In the left navigation menu, choose **Logs**.

- Choose **Log management**.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_2.png)

- In the search box, enter a keyword to find the Log management related to this project.

For example:

```txt
UploadInvoiceFileFunction
```

or:

```txt
/aws/lambda
```

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_3.png)

- Select the Log Group that you want to delete.

Example:

```txt
/aws/lambda/UploadInvoiceFileFunction
```

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_4.png)

- Choose **Actions**.

- Choose **Delete log group**.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_5.png)

- A confirmation dialog will appear.

- Choose **Delete** to confirm the deletion.

![Remove CloudWatch Log Groups](/images/7/7.7/Screenshot_6.png)

---

#### Delete the Remaining Log Groups

Repeat the same steps to delete the remaining Log Groups used by the system.

The Log Groups to check may include:

```txt
/aws/lambda/UploadInvoiceFileFunction
/aws/lambda/ProcessInvoiceFunction
/aws/lambda/InvoiceManagementFunction
```

If API Gateway logging was enabled, also check for Log Groups related to:

```txt
PostInvoiceAPI
GetInvoiceAPI
API-Gateway-Execution-Logs
```

{{% notice info %}}
If API Gateway logging was not enabled, you may not see separate API Gateway Log Groups. In that case, you only need to remove the Lambda Log Groups.
{{% /notice %}}

---

#### Review Log Streams Before Deleting

Before deleting a Log Group, you can open it to review the **Log Streams** inside.

Log Streams may contain useful information such as:

- Lambda invocation time.
- Requests received from API Gateway.
- Upload processing errors.
- Amazon Textract processing errors.
- OpenAI API request or response errors.
- DynamoDB read or write errors.
- CORS errors.
- Missing environment variables.
- Runtime or permission errors.

If you need these logs for your report, take screenshots or copy the important log messages before deleting the Log Group.

---

#### Verify the Deletion

After deleting the Log Groups:

- Return to the **Log groups** page in CloudWatch.
- Search for the deleted Log Group names again.
- Confirm that the Log Groups no longer appear.
- If the Lambda functions have already been deleted, CloudWatch will not create new logs for them.
- If a Lambda function still exists and is invoked again, AWS may automatically recreate the Log Group.

{{% notice warning %}}
If a Lambda function is still active, its CloudWatch Log Group may be recreated the next time the function is invoked. To fully clean up logs, delete the Lambda functions first or make sure they are no longer being called.
{{% /notice %}}

---

#### Impact on the System

After CloudWatch Log Groups are deleted, the system loses its previous log history.

| Component | Impact |
|---|---|
| Lambda Functions | Previous logs are no longer available for debugging. |
| API Gateway | Previous API Gateway logs are removed if logging was enabled. |
| React Frontend | Not directly affected. |
| Amazon S3 | Not directly affected. |
| Amazon Textract | Not directly affected. |
| OpenAI API | Not directly affected, but backend logs related to OpenAI requests are removed. |
| Amazon DynamoDB | Not directly affected. |

---

#### Important Notes

{{% notice info %}}
Deleting CloudWatch Log Groups does not delete Lambda functions, API Gateway APIs, S3 buckets, DynamoDB tables, Cognito User Pools, or Amplify Hosting Apps. This step only removes stored log data.
{{% /notice %}}

{{% notice warning %}}
If you need logs as evidence for your report or final submission, save screenshots or export important log messages before deleting them.
{{% /notice %}}

{{% notice info %}}
To prevent new logs from being generated, make sure the related Lambda functions and API Gateway resources have already been deleted or are no longer being invoked.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the Amazon CloudWatch Log Groups created for the **Serverless AI Invoice Scanner** system.

- The Lambda Log Groups have been deleted.
- API Gateway logs have been deleted if API Gateway logging was enabled.
- Previous log history is no longer stored in CloudWatch.
- This cleanup step helps remove unnecessary log data and reduce potential CloudWatch log storage costs.
