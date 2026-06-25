---
title: "Login with IAM User"
weight: 3
chapter: false
pre: " <b> 2.3 </b> "
---

#### Objective

In this step, you will log in using the **IAM User** that has been granted permissions to use the AWS Console. This is a mandatory step before deploying any resources.

---

#### Step 1: Open the browser in Incognito mode

-   Open Google Chrome (or your current browser) in **Incognito mode**.
-   Quick shortcut: Press **Ctrl + Shift + N**.
-   This prevents session conflicts if you're logged into multiple AWS accounts.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/001-loginwithiamuser.png)

---

#### Step 2: Open the Excel file containing IAM User credentials

-   Open the Excel file you downloaded from the system administrator.
-   In this file, you'll find the following information:

    -   **Account ID**
    -   **IAM User name**
    -   **Password**

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/002-loginwithiamuser.png)

{{% notice info %}}
This information is usually provided to you via email or as an attachment from the project manager.
{{% /notice %}}

---

#### Step 3: Visit the IAM login page

-   Go to the IAM login URL in the following format:

    ```
    https://<ACCOUNT_ID>.signin.aws.amazon.com/console
    ```

    Replace `<ACCOUNT_ID>` with the AWS account ID provided to you (e.g., `123456789012`).

---

#### Step 4: Enter login credentials

-   **IAM user name**: Enter the correct IAM username from the Excel file.
-   **Password**: Enter the corresponding password.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/003-loginwithiamuser.png)

-   Then click the **Sign in** button to access the system.

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/005-loginwithiamuser.png)

---

#### Step 5: Set Region to N. Virginia

-   After successfully logging into the AWS Management Console:

    -   Look at the **top-right corner** of the screen.
    -   Click on the current Region name.
    -   Select **N. Virginia** (us-east-1).

![IAM User](/images/2.environmentsetup/2.3-loginwithiamuser/004-loginwithiamuser.png)

{{% notice info %}}
All system resources will be deployed in the **us-east-1 (N. Virginia)** Region. Make sure you are operating in the correct region.
{{% /notice %}}

---

#### Conclusion

You have successfully logged in as an IAM User. In the next step, you will create an S3 Bucket in the **us-east-1** Region.
