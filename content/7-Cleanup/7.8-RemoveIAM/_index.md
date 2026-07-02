+++
title = "Remove IAM Roles and Policies"
date = 2025
weight = 8
chapter = false
pre = "<b>7.8 </b>"
+++

#### Overview

In this step, you will remove the **IAM Roles** and **IAM Policies** that were created for the **Serverless AI Invoice Scanner** system.

AWS Identity and Access Management (IAM) was used to grant permissions to different AWS services in this project. For example, Lambda functions required permissions to upload files to Amazon S3, read files from S3, call Amazon Textract, read and write data in DynamoDB, and write logs to Amazon CloudWatch.

After completing the lab and deleting the main resources such as API Gateway, Lambda, S3, DynamoDB, Cognito, and Amplify Hosting, you should review and delete IAM resources that are no longer used.

{{% notice warning %}}
Do not delete an IAM Role or IAM Policy unless you are sure it is no longer used by any other AWS service. Deleting the wrong IAM resource may cause Lambda functions or other AWS services to lose access permissions.
{{% /notice %}}

---

#### Resources to Review

In this project, IAM Roles were mainly used as execution roles for AWS Lambda functions.

The roles may have names similar to:

```txt
UploadInvoiceFileFunction-role
FetchInvoiceDetailsFunction-role
```

or AWS-generated names such as:

```txt
UploadInvoiceFileFunction-role-xxxxxxxx
FetchInvoiceDetailsFunction-role-xxxxxxxx
```

The related IAM policies may grant permissions to the following services:

| Service | Permission Purpose |
|---|---|
| Amazon S3 | Upload, read, or retrieve invoice files from the S3 bucket. |
| Amazon DynamoDB | Read, write, update, and query invoice data. |
| Amazon Textract | Call OCR services to extract text from invoice documents. |
| Amazon CloudWatch Logs | Write logs when Lambda functions are invoked. |
| AWS Lambda | Allow Lambda functions to run with the assigned execution role. |

{{% notice info %}}
The actual IAM Role and Policy names in your AWS account may be different depending on how you created the Lambda functions and permissions.
{{% /notice %}}

---

#### Check the IAM Role Attached to a Lambda Function

Before deleting an IAM Role, you should identify which role was attached to each Lambda function in the project.

- Open the **AWS Management Console**.

- Search for and open the **Lambda** service.

![Remove IAM Roles](/images/7/7.8/Screenshot_1.png)

- Open a Lambda function from this project, for example:

```txt
UploadInvoiceFileFunction
```

- Choose the **Configuration** tab.

- Choose **Permissions**.

![Remove IAM Roles](/images/7/7.8/Screenshot_2.png)

- Note the name of the **Execution role**.

Example:

```txt
UploadInvoiceFileFunction-role-xxxxxxxx
```
![Remove IAM Roles](/images/7/7.8/Screenshot_3.png)

- Repeat this step for the remaining Lambda functions if they have not already been deleted.

---

#### Delete an IAM Role

- Open the **AWS Management Console**.

- Search for and open the **IAM** service.

![Remove IAM Roles](/images/7/7.8/Screenshot_4.png)

- In the left navigation menu, choose **Roles**.

![Remove IAM Roles](/images/7/7.8/Screenshot_5.png)

- In the search box, enter a role name or keyword related to the project.

For example:

```txt
UploadInvoiceFileFunction
```

or:

```txt
invoice
```

![Remove IAM Roles](/images/7/7.8/Screenshot_6.png)

- Select the IAM Role that belongs to this project.

Example:

```txt
UploadInvoiceFileFunction-role-xxxxxxxx
```

![Remove IAM Roles](/images/7/7.8/Screenshot_7.png)

- Review the role details and attached permissions.

- Make sure the role is only used by the **Serverless AI Invoice Scanner** project.

- Choose **Delete**.

![Remove IAM Roles](/images/7/7.8/Screenshot_8.png)

- A confirmation dialog will appear.

- Enter the required confirmation text if the AWS Console asks for it.

- Choose **Delete** to remove the IAM Role.

![Remove IAM Roles](/images/7/7.8/Screenshot_9.png)

---

#### Delete the Remaining IAM Roles

Repeat the same steps to delete the remaining IAM Roles used by the project.

The roles to check may include:

```txt
UploadInvoiceFileFunction-role
ProcessInvoiceFunction-role
InvoiceManagementFunction-role
```

If you created a separate role for API Gateway or another service, review it and delete it only if it is no longer used.

---

#### Delete Customer Managed Policies

If you created **customer managed policies** for this project, you should also delete them after they are no longer attached to any IAM entity.

Example policy names may include:

```txt
InvoiceScannerS3Policy
InvoiceScannerDynamoDBPolicy
InvoiceScannerTextractPolicy
InvoiceScannerLambdaPolicy
```

To delete a customer managed policy:

- In the **IAM** service, choose **Policies** from the left navigation menu.

![Remove IAM Policies](/images/7/7.8/Screenshot_10.png)

- Search for policies related to the project using keywords such as:

```txt
AIInvoiceScannerFullPolicy
AmplifyAdminPolicy
```

![Remove IAM Policies](/images/7/7.8/Screenshot_11.png)

- Select the policy you want to delete.

- Open the **Entities attached** tab.

- Make sure the policy is not attached to any role, user, or group.

![Remove IAM Policies](/images/7/7.8/Screenshot_12.png)

- If the policy is still attached to an entity, detach it first.

- Choose **Delete**.

![Remove IAM Policies](/images/7/7.8/Screenshot_13.png)

- Confirm the deletion.

---

#### Do Not Delete AWS Managed Policies

Some policies are **AWS managed policies**, for example:

```txt
AWSLambdaBasicExecutionRole
AmazonS3FullAccess
AmazonDynamoDBFullAccess
AmazonTextractFullAccess
```

These policies are managed by AWS and may be used by other services or projects.

{{% notice warning %}}
Do not try to delete AWS managed policies. You only need to detach them from project-specific roles or delete the project-specific IAM Role if it is no longer used.
{{% /notice %}}

---

#### Review the IAM User Used for Deployment

During the deployment process, you may have created an IAM User for AWS CLI, Amplify Hosting, or local configuration.

Example:

```txt
ai-invoice-scanner-user
```

If this IAM User was created only for this lab and is no longer needed, you can delete its access keys or delete the user.

To review the IAM User:

- In the **IAM** service, choose **Users**.

- Search for the user used during deployment.

![Remove IAM User](/images/7/7.8/Screenshot_14.png)

- Open the user and choose the **Security credentials** tab.

- If the user is no longer needed, delete the **Access keys** first.

![Remove IAM User](/images/7/7.8/Screenshot_15.png)

- Then delete the IAM User if you are sure it is not used by any other project.

{{% notice warning %}}
Only delete an IAM User if you are sure it was created specifically for this lab. Do not delete an admin user or a user that is still used by other projects.
{{% /notice %}}

---

#### Verify the Deletion

After deleting IAM Roles and Policies:

- Return to the **Roles** list in IAM.
- Search for roles related to the project.
- Confirm that roles such as `UploadInvoiceFileFunction-role`, `ProcessInvoiceFunction-role`, and `InvoiceManagementFunction-role` no longer appear.
- Check the **Policies** list if you created customer managed policies.
- Check the **Users** list if you created a separate IAM User for this lab.
- Make sure no unused access keys remain active.

---

#### Impact on the System

After removing IAM Roles and Policies, any remaining services that depended on those permissions will no longer be able to access the required AWS resources.

| Component | Impact |
|---|---|
| Lambda Functions | May fail if their execution role has been deleted. |
| Amazon S3 | Not deleted, but Lambda can no longer access it if the role is removed. |
| Amazon DynamoDB | Not deleted, but Lambda can no longer read or write data if the role is removed. |
| Amazon Textract | Not deleted, but Lambda can no longer call Textract if the role is removed. |
| CloudWatch Logs | Existing logs remain, but Lambda can no longer write logs if the role is removed. |
| API Gateway | Not directly affected, but backend Lambda functions may fail if permissions are missing. |
| Amplify Hosting | Not directly affected. |
| Amazon Cognito | Not directly affected. |

---

#### Important Notes

{{% notice info %}}
IAM Roles should usually be deleted after the Lambda functions that depend on them have already been deleted.
{{% /notice %}}

{{% notice warning %}}
Do not delete IAM Roles or Policies that are used by services outside this lab. Always review the role name, attached permissions, trusted entities, and attached resources before deleting.
{{% /notice %}}

{{% notice info %}}
If you stored the OpenAI API key in AWS Secrets Manager, an IAM Role may have permission to read that secret. After deleting Lambda and IAM resources, you should also review Secrets Manager if you want to clean up all optional resources.
{{% /notice %}}

---

#### Conclusion

You have successfully reviewed and removed the IAM Roles, IAM Policies, or IAM User that are no longer used by the **Serverless AI Invoice Scanner** system.

- The Lambda execution roles used by the project have been deleted.
- Unused customer managed policies have been removed.
- The IAM User or access keys created only for this lab can be deleted if they are no longer needed.
- Unused permissions have been removed from the AWS account.
- This cleanup step helps reduce security risks and keeps the AWS account easier to manage.
