+++
title = "Remove Amplify Hosting App"
date = 2025
weight = 6
chapter = false
pre = "<b>7.6 </b>"
+++

#### Overview

In this step, you will delete the **AWS Amplify Hosting App** that was created to deploy the React frontend for the **Serverless AI Invoice Scanner** system.

AWS Amplify Hosting was used to build, deploy, and host the frontend application. After completing the lab, you should remove the Amplify Hosting App if the website is no longer needed.

{{% notice warning %}}
After deleting the Amplify Hosting App, the public frontend URL will no longer be accessible. Users will not be able to open the deployed web application.
{{% /notice %}}

---

#### Resource to Remove

In this project, the resource to remove is the frontend application deployed with **AWS Amplify Hosting**.

The application name may be similar to:

```txt
invoice-scanner-frontend
```

or the name you selected when creating the Amplify app.

This Amplify Hosting App was used for the following purposes:

| Component | Description |
|---|---|
| React Frontend App | Provides the invoice management dashboard. |
| Amplify Hosting Domain | Public frontend URL such as `https://main.xxxxx.amplifyapp.com`. |
| Build Settings | Defines how the React app is built using `npm install` and `npm run build`. |
| Environment Variables | Stores frontend build-time variables such as API Gateway endpoints, Cognito User Pool ID, and App Client ID. |
| GitHub Connection | Connects the GitHub repository to Amplify for automatic deployment. |

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **AWS Amplify** service.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_1.png)

- In the Amplify app list, select the frontend app used in this project.

Example:

```txt
invoice-scanner-frontend
```

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_2.png)

- After opening the app, choose **App settings** from the left navigation menu.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_3.png)

- Choose **General settings**.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_4.png)

- Scroll down to the **Delete app** section.

- Choose **Delete app**.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_5.png)

- A confirmation dialog will appear.

- Enter the app name as required by the AWS Console.

Example:

```txt
invoice-scanner-frontend
```

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_6.png)

- Choose **Delete** to remove the Amplify Hosting App.

![Remove Amplify Hosting App](/images/7/7.6/Screenshot_7.png)

---

#### Verify the Deletion

After deleting the Amplify Hosting App:

- Return to the application list in **AWS Amplify**.
- Confirm that the frontend app no longer appears.
- Open the previous Amplify Hosting URL.

Example:

```txt
https://main.xxxxx.amplifyapp.com
```

- The website should no longer be accessible.
- If the app was connected to GitHub, new commits pushed to the repository will no longer trigger automatic deployments.

---

#### Impact on the System

After the Amplify Hosting App is deleted, the deployed frontend website will no longer be available.

| Component | Impact |
|---|---|
| React frontend | The deployed website can no longer be accessed. |
| Amplify domain | The `amplifyapp.com` URL is removed. |
| GitHub auto deployment | New pushes to GitHub will no longer trigger Amplify deployments. |
| Environment variables | Amplify Hosting environment variables are deleted with the app. |
| Amazon Cognito | Not deleted directly, but callback URLs pointing to Amplify will no longer be valid. |
| API Gateway | Not directly affected. |
| Lambda Functions | Not directly affected. |
| Amazon S3 | Not directly affected. |
| Amazon DynamoDB | Not directly affected. |

---

#### Note About Cognito Callback URLs

If you added the Amplify Hosting URL to Amazon Cognito, for example:

```txt
https://main.xxxxx.amplifyapp.com/
```

inside:

```txt
Allowed callback URLs
Allowed sign-out URLs
```

then the URL will no longer be valid after the Amplify App is deleted.

If you still keep the Cognito User Pool for local testing, you can keep:

```txt
http://localhost:3000/
```

and remove the deleted Amplify Hosting URL from the Cognito configuration.

{{% notice info %}}
If the Cognito User Pool will also be deleted in the cleanup process, you do not need to update the callback URLs.
{{% /notice %}}

---

#### Note About the GitHub Repository

Deleting the Amplify Hosting App does **not** delete your GitHub repository.

After deleting the Amplify App:

- The source code remains in GitHub.
- The repository can still be reused for future deployments.
- If you want to delete the source code, you must delete the repository directly from GitHub.

{{% notice info %}}
Amplify Hosting only removes the deployed application from AWS. It does not automatically remove your source code from GitHub.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the AWS Amplify Hosting App used in the **Serverless AI Invoice Scanner** system.

- The React frontend app deployed on Amplify Hosting has been deleted.
- The public frontend URL is no longer accessible.
- Build settings, environment variables, and the GitHub connection in Amplify Hosting have been removed.
- Cognito, API Gateway, Lambda, S3, and DynamoDB are not automatically deleted by this step.
- This cleanup step helps remove unused frontend hosting resources from the AWS account.
