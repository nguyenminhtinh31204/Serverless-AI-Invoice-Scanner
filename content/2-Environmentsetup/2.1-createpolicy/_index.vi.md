---
title: "Tạo IAM Policies"
weight: 1
chapter: false
pre: " <b> 2.1 </b> "
---

#### Tổng quan

Trong bước này, bạn sẽ tạo 2 IAM Policy cần thiết cho hệ thống:

-   **AIInvoiceScannerFullPolicy**: cấp quyền đầy đủ cho các dịch vụ backend như Textract, Bedrock, Lambda, DynamoDB, API Gateway, Cognito và CloudWatch.
-   **AmplifyAdminPolicy**: cấp quyền toàn phần cho việc triển khai frontend sử dụng AWS Amplify.

---

#### Bước 1: Truy cập IAM Console

1. Truy cập [AWS Console](https://console.aws.amazon.com/), tìm **IAM**, sau đó chọn **IAM** trong kết quả.

![Policy](/images/2.environmentsetup/2.1-createpolicy/001-createpolicy.png)

2. Ở menu bên trái, chọn **Policies**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/002-createpolicy.png)

3. Nhấn nút **Create policy**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/003-createpolicy.png)

---

#### Bước 2: Tạo policy AIInvoiceScannerFullPolicy

1. Trong trang tạo policy, chuyển sang tab **JSON**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/004-createpolicy.png)

2. Dán đoạn mã sau vào trình chỉnh sửa:

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

3. Nhấn **Next** để tiếp tục.

![Policy](/images/2.environmentsetup/2.1-createpolicy/006-createpolicy.png)

4. Điền thông tin chi tiết:

    - **Policy name**: `AIInvoiceScannerFullPolicy`
    - **Description**: `Full permissions for S3, Textract, Bedrock, Lambda, DynamoDB, API Gateway, Cognito, and CloudWatch`

![Policy](/images/2.environmentsetup/2.1-createpolicy/007-createpolicy.png)

5. Nhấn **Create policy** để hoàn tất.

![Policy](/images/2.environmentsetup/2.1-createpolicy/008-createpolicy.png)

---

#### Bước 3: Tạo policy AmplifyAdminPolicy

1. Quay lại danh sách **Policies**, nhấn **Create policy**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/009-createbonuspolicy.png)

2. Chuyển sang tab **JSON**, dán nội dung sau:

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

3. Nhấn **Next**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/011-createbonuspolicy.png)

4. Điền thông tin chi tiết:

    - **Policy name**: `AmplifyAdminPolicy`
    - **Description**: `Full access for Amplify frontend and backend`

![Policy](/images/2.environmentsetup/2.1-createpolicy/012-createbonuspolicy.png)

5. Nhấn **Create policy**.

![Policy](/images/2.environmentsetup/2.1-createpolicy/013-createbonuspolicy.png)

---

#### Bước 4: Kiểm tra lại policies

1. Quay về danh sách **Policies**.
2. Tìm kiếm và xác nhận hai policy đã được tạo:

    - `AIInvoiceScannerFullPolicy`
    - `AmplifyAdminPolicy`

![Policy](/images/2.environmentsetup/2.1-createpolicy/014-checkpolicy.png)
![Policy](/images/2.environmentsetup/2.1-createpolicy/015-checkpolicy.png)

{{% notice info %}}
Nếu bạn không thấy policies vừa tạo, hãy thử làm mới trình duyệt hoặc kiểm tra lại tên nhập vào.
{{% /notice %}}
