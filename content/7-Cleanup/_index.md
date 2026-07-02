+++
title = "Resource Cleanup"
date = 2025
weight = 7
chapter = false
pre = "<b>7. </b>"
+++

#### Overview

After completing the deployment and testing of the **Serverless AI Invoice Scanner** system, you need to delete the AWS resources created during the lab.

Resource cleanup helps:

- Avoid unnecessary costs.

- Free up unused AWS services.

- Keep your AWS account tidy and easy to manage.

- Ensure that test resources do not continue running after the lab is finished.

In this section, you will delete the main resources created in the system, including:

- Amazon API Gateway.

- AWS Lambda Functions.

- Amazon S3 Bucket.

- Amazon DynamoDB Table.

- Amazon Cognito User Pool.

- AWS Amplify Hosting App.
- Amazon CloudWatch Log Groups.

- IAM Roles and Policies.

- Other optional resources such as AWS Secrets Manager, Parameter Store, CloudFormation, or CloudWatch Alarms, if used.

{{% notice warning %}}
After deleting resources, related data may not be recoverable. Please ensure you have backed up necessary data before performing the deletion.

{{% /notice %}}

#### Recommended Cleanup Order

Resources should be deleted in the following order to avoid dependency errors between services:

1. [Delete API Gateway](7.1-RemoveAPIGateway/)

2. [Delete Lambda Functions](7.2-RemoveLambdaFunction/)

3. [Delete S3 Bucket](7.3-RemoveS3Bucket/)

4. [Delete DynamoDB Table](7.4-RemoveDynamoDB/)

5. [Delete Cognito User Pool](7.5-RemoveCognitoUserPool/)

6. [Delete Amplify Hosting App](7.6-RemoveAmplifyHosting/)

7. [Delete CloudWatch Log Groups](7.7-RemoveCloudWatch/)

8. [Delete IAM Roles and Policies](7.8-RemoveIAM/)

9. [Remove other optional resources if any](7.9-CheckRemainingResources/)

{{% notice info %}}
Some services like Amazon Textract don't need to remove their own resources because in this project Textract is called directly from Lambda and doesn't create fixed resources that need managing.

{{% /notice %}}
