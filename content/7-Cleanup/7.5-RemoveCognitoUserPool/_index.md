+++
title = "Remove Cognito User Pool"
date = 2025
weight = 5
chapter = false
pre = "<b>7.5 </b>"
+++

#### Overview

In this step, you will delete the **Amazon Cognito User Pool** that was created for the **Serverless AI Invoice Scanner** system.

Amazon Cognito was used to provide user authentication for the React frontend application. After the lab is completed, the Cognito User Pool should be removed to avoid keeping unused authentication resources in the AWS account.

{{% notice warning %}}
Deleting a Cognito User Pool will permanently remove all users, app clients, authentication settings, and related configuration in that user pool. Make sure you no longer need the user accounts before deleting it.
{{% /notice %}}

---

#### Resources to Remove

In this project, the Cognito resource to remove is:

```txt
invoice-scanner-user-pool
```

This user pool was used by the frontend application together with the following environment variables:

```env
REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

After the user pool is deleted, the frontend login and sign-up features will no longer work.

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **Amazon Cognito** service.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_1.png)

- In the left navigation menu, choose **User pools**.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_2.png)

- From the list of user pools, select the user pool used in this project:

```txt
invoice-scanner-user-pool
```

![Remove Cognito User Pool](/images/7/7.5/Screenshot_3.png)

- After opening the user pool, choose **Delete user pool**.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_4.png)

- A confirmation dialog will appear.

- Enter the required confirmation text to confirm the deletion. Depending on the AWS Console interface, you may need to type:

```txt
delete
```

or the exact user pool name:

```txt
invoice-scanner-user-pool
```

![Remove Cognito User Pool](/images/7/7.5/Screenshot_5.png)

- Choose **Delete** to permanently remove the Cognito User Pool.

![Remove Cognito User Pool](/images/7/7.5/Screenshot_6.png)

---

#### Verify the Deletion

After deleting the user pool:

- Return to the **User pools** list.
- Confirm that `invoice-scanner-user-pool` no longer appears in the list.
- Open the deployed frontend application.
- The Cognito authentication screen should no longer work because the User Pool ID and App Client ID are no longer valid.

---

#### Impact on the System

After the Cognito User Pool is deleted:

| Component | Impact |
|---|---|
| React frontend | Users can no longer sign in or sign up. |
| Cognito Authenticator | The login screen will fail because the User Pool no longer exists. |
| App Client ID | The app client is deleted together with the user pool. |
| Existing users | All registered users are permanently removed. |
| API Gateway | API Gateway is not directly affected unless a Cognito Authorizer was configured. |
| Lambda Functions | Lambda functions are not directly affected. |
| DynamoDB | Invoice data remains in DynamoDB unless the table is deleted separately. |

---

#### Important Notes

{{% notice info %}}
If your API Gateway is configured with a Cognito Authorizer, delete or detach the authorizer before removing the Cognito User Pool. Otherwise, API routes that depend on the authorizer may stop working or show authorization errors.
{{% /notice %}}

{{% notice warning %}}
Do not delete the Cognito User Pool if you still want to test the frontend authentication feature. Once deleted, the user pool and its users cannot be restored.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the Amazon Cognito User Pool used in the **Serverless AI Invoice Scanner** system.

- The user pool `invoice-scanner-user-pool` has been deleted.
- The app client used by the React frontend has also been removed.
- User sign-up and sign-in are no longer available.
- The frontend authentication configuration is no longer valid.
- This cleanup step helps remove unused authentication resources from the AWS account.
