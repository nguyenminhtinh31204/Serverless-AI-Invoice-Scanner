+++
title = "Remove IAM User and Policy"
date = 2025
weight = 6
chapter = false
pre = "<b>7.6 </b>"
+++

{{% notice warning %}}
⚠️ **Note**: Please log in with **root user** to remove IAM service and Policy.
{{% /notice %}}

#### Steps

-   Open **AWS Management Console** → Search for **IAM** service.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_1.png)

-   In the left navigation bar, select **Users**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_2.png)

-   Find and select the user `ai-invoice-scanner-user` to go to the user's details page.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_3.png)

-   Scroll down to the **Permissions policies** section, select all the policies assigned to the user, then click the **Remove** button.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_4.png)

-   Click **Remove policies** to confirm removing the policies from the user.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_5.png)

-   Access the **Security credentials** tab.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_6.png)

-   Scroll down to **Access keys**, click **Actions** → **Delete** to delete the access key.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_7.png)

-   After deleting the access key, scroll to the top of the page → Click **Delete** to delete the IAM User.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_8.png)

-   During the deletion process, click **Deactivate access key** if prompted.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_9.png)

-   In the confirmation window, enter `confirm` → Click **Delete user**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_10.png)

-   After deleting the user, in the left navigation bar, select **Policies**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_11.png)

-   Find the policy `AIInvoiceScannerFullPolicy` → Click **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_12.png)

-   Re-enter the policy name to confirm → Click **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_13.png)

-   Repeat the above steps to delete the policy `AmplifyAdminPolicy`.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_14.png)

-   Re-enter the policy name to confirm → Click **Delete**.

![Remove IAM User and Policy](/images/7/7.6/Screenshot_15.png)

{{% notice tip %}}
ℹ️ To be careful, please check the services related to the project again to avoid **incurring costs** later!
{{% /notice %}}
