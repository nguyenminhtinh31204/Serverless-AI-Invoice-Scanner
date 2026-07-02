+++
title = "Check Remaining Resources"
date = 2025
weight = 9
chapter = false
pre = "<b>7.9 </b>"
+++

#### Overview

In this final cleanup step, you will check optional or secondary resources that may still exist after deleting the main components of the **Serverless AI Invoice Scanner** system.

The previous cleanup steps removed the main project resources:

- API Gateway
- Lambda Functions
- S3 Bucket
- DynamoDB Table
- Cognito User Pool
- Amplify Hosting App
- CloudWatch Log Groups
- IAM Roles and Policies

{{% notice info %}}
Section 7.9 is used to verify optional resources that may have been created depending on how you deployed or tested the project.
{{% /notice %}}

---

#### Resources to Check

After deleting the main resources, only check the optional resources below if you actually used them during the project.

| Resource | When to check |
|---|---|
| AWS Secrets Manager | If you stored the OpenAI API Key or other secrets in AWS. |
| Systems Manager Parameter Store | If you stored configuration values or API keys in Parameter Store. |
| CloudFormation Stack | If you deployed resources using a template, AWS SAM, or CloudFormation stack. |
| CloudWatch Alarms | If you created alarms for Lambda, API Gateway, or billing. |
| Resource Groups Tag Editor | If you used tags to manage project resources. |
| Billing / Cost Explorer | To confirm there are no unexpected AWS charges. |

---

#### Check AWS Secrets Manager

If you stored the **OpenAI API Key** in AWS Secrets Manager, check and delete the secret if it is no longer needed.

- Open the **AWS Management Console**.
- Search for and open **Secrets Manager**.
- Find secrets related to the project.

Example secret names:

```txt
openai-api-key
invoice-scanner-openai-secret
```

- Open the secret you want to check.
- Choose **Actions**.
- Choose **Delete secret**.
- Select the recovery window required by AWS.
- Confirm the secret deletion.

{{% notice warning %}}
Do not delete a secret if it is still being used by another project. The OpenAI API Key is sensitive information and should not be stored in frontend source code or a public repository.
{{% /notice %}}

---

#### Check Systems Manager Parameter Store

If you stored API keys or configuration values in **AWS Systems Manager Parameter Store**, check and delete any parameters that are no longer needed.

- Open the **AWS Management Console**.
- Search for and open **Systems Manager**.
- In the left navigation menu, choose **Parameter Store**.
- Find parameters related to the project.

Example parameter names:

```txt
/invoice-scanner/openai-api-key
/invoice-scanner/api-url
/invoice-scanner/s3-bucket
```

- Select the parameter that is no longer used.
- Choose **Delete**.

{{% notice info %}}
If the project does not use Parameter Store, you can skip this step.
{{% /notice %}}

---

#### Check CloudFormation Stack

If you deployed resources using **AWS CloudFormation**, **AWS SAM**, or another infrastructure template, check whether any stack still exists.

- Open the **AWS Management Console**.
- Search for and open **CloudFormation**.
- Choose **Stacks**.
- Find stacks related to the project.

Example stack names:

```txt
serverless-ai-invoice-scanner
invoice-scanner-stack
```

- If the stack is no longer needed, select the stack and choose **Delete**.

{{% notice warning %}}
Only delete a CloudFormation stack if you are sure it was created for this lab. Deleting a stack may remove multiple resources at the same time.
{{% /notice %}}

---

#### Check CloudWatch Alarms

If you created **CloudWatch Alarms** to monitor Lambda, API Gateway, or billing, delete alarms that are no longer used.

- Open the **CloudWatch** service.
- Choose **Alarms**.
- Choose **All alarms**.
- Find alarms related to the project.

Example alarm names:

```txt
InvoiceScannerLambdaErrorAlarm
InvoiceScannerAPIGateway5xxAlarm
```

- Select the alarm that is no longer used.
- Choose **Actions**.
- Choose **Delete**.

---

#### Check Resources with Tag Editor

If you added tags to project resources, you can use **Tag Editor** to quickly find remaining resources.

Example tag:

```txt
Project = ServerlessAIInvoiceScanner
```

How to use Tag Editor:

- Open **AWS Resource Groups & Tag Editor**.
- Choose **Tag Editor**.
- Select the AWS Region where you deployed the project.
- Search by tag or resource type.
- Check whether any resources related to the project still exist.

{{% notice info %}}
Tag Editor is most useful if you added tags during deployment. If you did not use tags, manually check the main AWS services instead.
{{% /notice %}}

---

#### Check Billing and Cost Explorer

After cleanup, you should check billing information to confirm that no unexpected services are still generating costs.

- Open **AWS Billing and Cost Management**.
- Choose **Bills** or **Cost Explorer**.
- Review the services that are generating costs.
- Pay attention to services such as:

```txt
Amazon S3
AWS Lambda
Amazon DynamoDB
Amazon API Gateway
Amazon CloudWatch
Amazon Cognito
AWS Secrets Manager
AWS Systems Manager
```

{{% notice warning %}}
Some billing data may take several hours or days to update. If you recently deleted resources, check again after some time.
{{% /notice %}}

---

#### Final Cleanup Checklist

You can use the checklist below to confirm that the cleanup process is complete.

| Check item | Status |
|---|---|
| API Gateway has been deleted | ☐ |
| Lambda Functions have been deleted | ☐ |
| S3 Bucket has been emptied and deleted | ☐ |
| DynamoDB Table has been deleted | ☐ |
| Cognito User Pool has been deleted | ☐ |
| Amplify Hosting App has been deleted | ☐ |
| CloudWatch Log Groups have been deleted | ☐ |
| Unused IAM Roles and Policies have been deleted | ☐ |
| Secrets Manager has been checked | ☐ |
| Parameter Store has been checked | ☐ |
| CloudFormation Stack has been checked | ☐ |
| CloudWatch Alarms have been checked | ☐ |
| Tag Editor has been checked if tags were used | ☐ |
| Billing / Cost Explorer has been checked | ☐ |

---

#### Impact After Cleanup

After completing the cleanup process, the **Serverless AI Invoice Scanner** system will no longer run on AWS.

| Component | Status after cleanup |
|---|---|
| Frontend | The website is no longer deployed on Amplify. |
| Authentication | The Cognito User Pool has been deleted. |
| API | API Gateway has been deleted. |
| Backend | Lambda Functions have been deleted. |
| File storage | The S3 Bucket has been deleted. |
| Database | The DynamoDB Table has been deleted. |
| Logs | CloudWatch Logs have been deleted. |
| Permissions | Unused IAM Roles and Policies have been deleted. |
| Optional resources | Secrets Manager, Parameter Store, CloudFormation, CloudWatch Alarms, Tag Editor, and Billing have been checked if used. |

---

#### Important Notes

{{% notice info %}}
Not every deployment uses all optional resources listed in Section 7.9. You only need to delete the resources that were actually created for this lab.
{{% /notice %}}

{{% notice warning %}}
Do not delete a resource if you are not sure it belongs to this project. Accidentally deleting resources may affect other applications or labs in the same AWS account.
{{% /notice %}}

{{% notice info %}}
To avoid future charges, check AWS Billing again after completing the cleanup process.
{{% /notice %}}

---

#### Conclusion

You have completed the final verification step after cleaning up the **Serverless AI Invoice Scanner** resources.

- The main project resources have been deleted.
- Optional resources such as Secrets Manager, Parameter Store, CloudFormation, and CloudWatch Alarms have been checked if used.
- Tag Editor can be used to review remaining resources if the project used tags.
- Billing has been reviewed to detect any unusual costs.
- This final step helps keep the AWS account clean and reduces the risk of unexpected charges.
