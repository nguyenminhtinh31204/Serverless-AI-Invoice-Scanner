---
title: "Create IAM Role for Lambda"
weight: 1
chapter: false
pre: " <b> 3.1 </b> "
---

#### Overview

In this step, you will create a **dedicated IAM Role** so that Lambda functions in the **Serverless Invoice Scanner** system can access AWS services such as **Amazon Textract**, **Amazon Bedrock**, **Amazon DynamoDB**, **Amazon S3**, and **CloudWatch Logs**.

---

#### Step 1: Access the IAM Console

1. Open the [AWS Management Console](https://console.aws.amazon.com/) in **Incognito mode** to avoid session conflicts if you're logged in with multiple accounts.

2. Search for and select **IAM** in the search bar.

![Open IAM](/images/3.lambdafunctions/3.1-iamrole/001-openiam.png)

3. In the left navigation pane, choose **Roles**, then click **Create role**.

![Create Role](/images/3.lambdafunctions/3.1-iamrole/002-createrole.png)

---

#### Step 2: Configure Role for Lambda

1. **Trusted entity type**: select **AWS service**
2. **Use case**: select **Lambda**

![Select Lambda Use Case](/images/3.lambdafunctions/3.1-iamrole/003-selectlambda.png)

3. Click **Next** to proceed.

![Click Next](/images/3.lambdafunctions/3.1-iamrole/003-clicknext.png)

---

#### Step 3: Attach Permissions to the IAM Role

1. In the **Add permissions** step, search for and check the following policies:

    - `AmazonS3FullAccess`
    - `AmazonDynamoDBFullAccess`
    - `AmazonTextractFullAccess`
    - `AmazonBedrockFullAccess`
    - `AWSLambdaBasicExecutionRole`

![AmazonS3FullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazons3fullaccess.png)  
![AmazonDynamoDBFullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazondynamodbfullaccess.png)  
![AmazonTextractFullAccess](/images/3.lambdafunctions/3.1-iamrole/004-amazontextractfullaccess.png)  
![AWSLambdaBasicExecutionRole](/images/3.lambdafunctions/3.1-iamrole/004-amazonlambdabasicexecutionrole.png)

2. Click **Next** to continue.

![Click Next](/images/3.lambdafunctions/3.1-iamrole/004-clicknext.png)

---

#### Step 4: Name and Finish

1. **Role name**: `LambdaExecutionRole-AIInvoiceScanner`
2. **Description**: `Role for Lambda to access S3, Textract, Bedrock, DynamoDB, and CloudWatch`

![Name Role](/images/3.lambdafunctions/3.1-iamrole/005-namerole.png)

3. Click **Create role** to finish.

![Create Role](/images/3.lambdafunctions/3.1-iamrole/005-createrole.png)

4. After creation, go to the **Roles** section in the **IAM Console**. You will see the `LambdaExecutionRole-AIInvoiceScanner` listed.

![Check Role Details](/images/3.lambdafunctions/3.1-iamrole/006-checkrole.png)
