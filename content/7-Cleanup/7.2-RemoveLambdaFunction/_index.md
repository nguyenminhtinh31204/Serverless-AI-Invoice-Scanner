+++
title = "Remove Lambda Functions"
date = 2025
weight = 2
chapter = false
pre = "<b>7.2 </b>"
+++

#### Overview

In this step, you will delete the **AWS Lambda Functions** that were created for the **Serverless AI Invoice Scanner** system.

AWS Lambda was used to run the backend logic of the system. The Lambda functions handled invoice upload requests, stored invoice files in Amazon S3, processed invoices with Amazon Textract, called the OpenAI API to normalize extracted data, and managed invoice records in Amazon DynamoDB.

After completing the lab, you should remove unused Lambda functions to avoid keeping unnecessary backend resources in your AWS account.

{{% notice warning %}}
After a Lambda function is deleted, any API Gateway route, S3 event notification, or other trigger connected to that function will no longer work. Make sure you no longer need the function before deleting it.
{{% /notice %}}

---

#### Resources to Remove

In this project, the Lambda functions to remove may include:

| Lambda Function | Purpose |
|---|---|
| `UploadInvoiceFileFunction` | Receives invoice files from the frontend through API Gateway and uploads them to Amazon S3. |
| `ProcessInvoiceFunction` | Processes uploaded invoice files from S3, uses Amazon Textract to extract text, calls the OpenAI API to normalize invoice data, and stores the result in DynamoDB. |
| `InvoiceManagementFunction` | Handles invoice retrieval, invoice search, tag updates, and starred invoice updates. |

{{% notice info %}}
The actual Lambda function names in your AWS account may be slightly different. Choose the Lambda functions that were created for the Serverless AI Invoice Scanner project.
{{% /notice %}}

---

#### Check Lambda Triggers Before Deleting

Before deleting a Lambda function, review its triggers to understand which service is connected to it.

Common triggers in this project include:

| Trigger | Related Function |
|---|---|
| API Gateway `POST /uploads` | `UploadInvoiceFileFunction` |
| S3 Event Notification | `ProcessInvoiceFunction` |
| API Gateway `GET /invoice` | `InvoiceManagementFunction` |
| API Gateway `GET /invoice/{id}` | `InvoiceManagementFunction` |
| API Gateway `PATCH /invoice/tags/{id}` | `InvoiceManagementFunction` |
| API Gateway `PATCH /invoice/starred/{id}` | `InvoiceManagementFunction` |

If API Gateway was already deleted in the previous cleanup step, those API triggers are no longer needed.

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **Lambda** service.

![Remove Lambda Functions](/images/7/7.2/Screenshot_1.png)

- In the left navigation menu, choose **Functions**.

![Remove Lambda Functions](/images/7/7.2/Screenshot_2.png)

- In the function list, search for the Lambda function you want to delete.

Example:

```txt
UploadInvoiceFileFunction
```

![Remove Lambda Functions](/images/7/7.2/Screenshot_3.png)

- Select the Lambda function to open its details page.

- Review the function name, runtime, environment variables, permissions, and triggers to make sure you are deleting the correct function.

![Remove Lambda Functions](/images/7/7.2/Screenshot_4.png)

- Choose **Actions**.

- Choose **Delete function**.

![Remove Lambda Functions](/images/7/7.2/Screenshot_6.png)

- A confirmation dialog will appear.

- Enter the required confirmation text if the AWS Console asks for it.

In some cases, you may need to type:

```txt
delete
```

or the exact Lambda function name.

![Remove Lambda Functions](/images/7/7.2/Screenshot_5.png)

- Choose **Delete** to confirm the deletion.

---

#### Delete the Remaining Lambda Functions

Repeat the same steps to delete the remaining Lambda functions in the system.

The functions to check may include:

```txt
UploadInvoiceFileFunction
FetchInvoiceDetailsFunction
```

These functions correspond to the main backend workflows:

- `UploadInvoiceFileFunction`: handles invoice file uploads.
- `FetchInvoiceDetailsFunction`: processes invoices after they are uploaded to S3,handles invoice query and update APIs.

---

#### Verify the Deletion

After deleting the Lambda functions:

- Return to the **Functions** page in AWS Lambda.
- Search for the project Lambda function names.
- Confirm that the deleted functions no longer appear in the list.
- If you try to call the old API Gateway endpoints or S3 event triggers, they will no longer have a Lambda backend to execute.
- Check CloudWatch Logs if you need to confirm that no new invocations are being created.

---

#### Impact on the System

After the Lambda functions are deleted, the backend processing logic of the system will no longer work.

| Component | Impact |
|---|---|
| API Gateway | Cannot invoke the backend if the integrated Lambda function has been deleted. |
| React frontend | Upload, view, search, tag update, and starred update features will fail. |
| Amazon S3 | Existing invoice files remain if the bucket has not been deleted. |
| S3 Event Notification | No longer has a processing Lambda to handle new upload events. |
| Amazon Textract | No longer receives requests from the processing Lambda. |
| OpenAI API | No longer receives backend requests from Lambda. |
| Amazon DynamoDB | Existing invoice data remains if the table has not been deleted. |
| Amazon CloudWatch | Existing Lambda logs remain unless the Log Groups are deleted separately. |

---

#### Important Notes

{{% notice info %}}
Deleting a Lambda function does not automatically delete its CloudWatch Log Group. If you want to fully clean up the logs, delete the related CloudWatch Log Groups in the CloudWatch cleanup step.
{{% /notice %}}

{{% notice warning %}}
If a Lambda function stored sensitive values such as an OpenAI API key in environment variables, deleting the function removes those environment variables from Lambda. However, if the API key was stored in AWS Secrets Manager, you must delete the secret separately if it is no longer needed.
{{% /notice %}}

{{% notice info %}}
If the Lambda function is still connected to API Gateway or an S3 Event Notification, the trigger configuration may remain visible in the related service until that service is also cleaned up.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the AWS Lambda functions used in the **Serverless AI Invoice Scanner** system.

- The upload Lambda function has been deleted.
- The invoice processing Lambda function using Textract and the OpenAI API has been deleted.
- The invoice management Lambda function has been deleted.
- The serverless backend can no longer process requests from API Gateway or S3 events.
- This cleanup step helps remove unused compute resources from the AWS account.
