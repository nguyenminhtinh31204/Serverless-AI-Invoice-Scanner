---
title: "Create IAM Policies"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

#### Overview

In this step, you will create 2 essential IAM Policies for the system:

-   **AIInvoiceScannerFullPolicy**: grants full permissions to backend services such as Textract, Bedrock, Lambda, DynamoDB, API Gateway, Cognito, and CloudWatch.
-   **AmplifyAdminPolicy**: grants full access for frontend deployment using AWS Amplify.

---

#### Step 1: Access IAM Console

1. Go to [AWS Console](https://console.aws.amazon.com/), search for **IAM**, then select **IAM** from the results.

![Policy](/images/2.environmentsetup/2.1-createpolicy/001-createpolicy.png)

2. In the left-hand menu, click on **Policies**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/002-createpolicy.png)

3. Click the **Create policy** button.

![Policy](/images/2.environmentsetup/2.1-createpolicy/003-createpolicy.png)

---

#### Step 2: Create the AIInvoiceScannerFullPolicy

1. On the Create Policy page, switch to the **JSON** tab.

![Policy](/images/2.environmentsetup/2.1-createpolicy/004-createpolicy.png)

2. Paste the following code into the editor:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "S3FullAccess",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "*"
        },
        {
            "Sid": "TextractFullAccess",
            "Effect": "Allow",
            "Action": "textract:*",
            "Resource": "*"
        },
        {
            "Sid": "BedrockFullAccess",
            "Effect": "Allow",
            "Action": "bedrock:*",
            "Resource": "*"
        },
        {
            "Sid": "LambdaFullAccess",
            "Effect": "Allow",
            "Action": "lambda:*",
            "Resource": "*"
        },
        {
            "Sid": "LambdaBasicExecutionRole",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*"
        },
        {
            "Sid": "DynamoDBFullAccess",
            "Effect": "Allow",
            "Action": "dynamodb:*",
            "Resource": "*"
        },
        {
            "Sid": "APIGatewayAdmin",
            "Effect": "Allow",
            "Action": "apigateway:*",
            "Resource": "*"
        },
        {
            "Sid": "CognitoPowerUser",
            "Effect": "Allow",
            "Action": ["cognito-idp:*", "cognito-sync:*", "cognito-identity:*"],
            "Resource": "*"
        },
        {
            "Sid": "CloudWatchFullAccess",
            "Effect": "Allow",
            "Action": "cloudwatch:*",
            "Resource": "*"
        },
        {
            "Sid": "AllowIAMAccessForLambdaSetup",
            "Effect": "Allow",
            "Action": [
                "iam:DetachRolePolicy",
                "iam:GetPolicy",
                "iam:GetPolicyVersion",
                "iam:GetRole",
                "iam:ListRoles",
                "iam:PassRole",
                "iam:ListPolicies",
                "iam:AttachRolePolicy",
                "iam:ListRolePolicies",
                "iam:ListAttachedRolePolicies",
                "iam:PutRolePolicy",
                "iam:GetRolePolicy",
                "iam:ListInstanceProfilesForRole",
                "iam:RemoveRoleFromInstanceProfile",
                "iam:DeleteRole",
                "iam:DeleteRolePolicy",
                "iam:CreateRole",
                "iam:TagRole",
                "iam:UpdateRole"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowValidatePolicy",
            "Effect": "Allow",
            "Action": "access-analyzer:ValidatePolicy",
            "Resource": "*"
        },
        {
            "Sid": "CloudWatchLogsFullAccess",
            "Effect": "Allow",
            "Action": [
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:GetLogEvents",
                "logs:FilterLogEvents",
                "logs:DeleteLogGroup"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowDynamoDBAutoScalingDescribe",
            "Effect": "Allow",
            "Action": [
                "application-autoscaling:DescribeScalableTargets",
                "application-autoscaling:DescribeScalingPolicies"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AllowEC2VpcEndpointDescribe",
            "Effect": "Allow",
            "Action": "ec2:DescribeVpcEndpoints",
            "Resource": "*"
        }
    ]
}
```

![Policy](/images/2.environmentsetup/2.1-createpolicy/005-createpolicy.png)

3. Click **Next** to proceed.

![Policy](/images/2.environmentsetup/2.1-createpolicy/006-createpolicy.png)

4. Fill in the policy details:

    - **Policy name**: `AIInvoiceScannerFullPolicy`
    - **Description**: `Full permissions for S3, Textract, Bedrock, Lambda, DynamoDB, API Gateway, Cognito, and CloudWatch`

![Policy](/images/2.environmentsetup/2.1-createpolicy/007-createpolicy.png)

5. Click **Create policy** to complete.

![Policy](/images/2.environmentsetup/2.1-createpolicy/008-createpolicy.png)

---

#### Step 3: Create the AmplifyAdminPolicy

1. Return to the **Policies** list, click **Create policy**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/009-createbonuspolicy.png)

2. Switch to the **JSON** tab and paste the following content:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AmplifyAdminAccess",
                "Effect": "Allow",
                "Action": "amplify:*",
                "Resource": "*"
            },
            {
                "Sid": "AmplifyHostingAccess",
                "Effect": "Allow",
                "Action": ["amplifybackend:*", "amplifyuibuilder:*"],
                "Resource": "*"
            }
        ]
    }
    ```

![Policy](/images/2.environmentsetup/2.1-createpolicy/010-createbonuspolicy.png)

3. Click **Next**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/011-createbonuspolicy.png)

4. Enter the policy details:

    - **Policy name**: `AmplifyAdminPolicy`
    - **Description**: `Full access for Amplify frontend and backend`

![Policy](/images/2.environmentsetup/2.1-createpolicy/012-createbonuspolicy.png)

5. Click **Create policy**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/013-createbonuspolicy.png)

---

#### Step 4: Review the policies

1. Go back to the **Policies** list.
2. Search for and verify the two created policies:

    - `AIInvoiceScannerFullPolicy`
    - `AmplifyAdminPolicy`

![Policy](/images/2.environmentsetup/2.1-createpolicy/014-checkpolicy.png)
![Policy](/images/2.environmentsetup/2.1-createpolicy/015-checkpolicy.png)

{{% notice info %}}
If you don’t see the newly created policies, try refreshing your browser or double-check the names.
{{% /notice %}}
