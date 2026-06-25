---
title: "Create IAM User and Assign Permissions"
weight: 2
chapter: false
pre: " <b> 2.2 </b> "
---

#### Overview

In this step, you will create an IAM User with access to AWS services via the Command Line Interface (CLI) or SDK. This user will be assigned the two previously created policies to interact with the backend and frontend of the Serverless Invoices Scanner system.

---

#### Step 1: Access the IAM Console

1. Go to [AWS Console](https://console.aws.amazon.com/), search for **IAM**, then click on **IAM** in the results.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/001-createiamuser.png)

2. In the left-hand menu, select **Users**.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/002-createiamuser.png)

3. Click the **Create user** button to start creating a new IAM User.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/003-createiamuser.png)

---

#### Step 2: Configure IAM User

1. **User name**: `ai-invoice-scanner-user`
2. **Check the box**: Provide user access to the AWS Management Console.
3. **Select**: I want to create an IAM user.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/004-createiamuser.png)

4. Under **Console password**:

    - **Select**: Custom password.
    - **Set password**: `Admin@123`
    - **Uncheck**: Users must create a new password at next sign-in.

{{% notice info %}}
Unchecking this option allows the user to avoid being prompted to change the password on their first login.
{{% /notice %}}

5. Click **Next** to proceed to the permissions step.

![IAM User](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/005-createiamuser.png)

> 💡 _You can choose a different password according to your internal security policy._

---

#### Step 3: Attach Policies to the IAM User

1. In the **Set permissions** section, choose **Attach policies directly**.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/006-attachpolicy.png)

2. Search for and select the following policies:

    - `AIInvoiceScannerFullPolicy`
    - `AmplifyAdminPolicy`

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/007-attachpolicy.png)
![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/008-attachpolicy.png)

3. Click **Next** to continue.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/009-attachpolicy.png)

4. Click **Create user** to finish creating the IAM User.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/010-UPDATEattachpolicy.png)

---

#### Step 4: Save Login Information

1. Click **Download .csv file** to save the **Access Key ID** and console password.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/012-savefileiamuser.png)

2. The file will be downloaded as an Excel sheet — be sure to **store this file securely** for future use.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/013-savefileiamuser.png)

---

#### Step 5: Verify IAM User Information

1. Click **Return to users list** to go back to the Users list.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/014-checkiamuser.png)

2. The newly created IAM User will be listed as shown:

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/015-checkiamuser.png)

3. Click the user to view its detailed information.

![Attach Policy](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/016-checkiamuser.png)

---

#### Step 6: Create Access Key

1. On the user detail page, click **Create access key**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/017-createaccesskey.png)

2. Select **Command Line Interface (CLI)**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/018-createaccesskey.png)

3. **Check the box**: I understand the above recommendation and want to proceed to create an access key.
4. Click **Next** to proceed.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/019-createaccesskey.png)

---

#### Step 7: Add a Description for the Access Key

1. **Description tag value**: `AI Invoice Scanner Project`
2. Click **Create access key**.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/020-createaccesskey.png)

---

#### Step 8: Backup Access Key

1. Once the access key is successfully created, AWS will display:

    - **Access Key ID** ✅
    - **Secret Access Key** 🔐

{{% notice warning %}}
⚠️ **Note**: This is the only time you will see the **Secret Access Key**. Store it securely and never share it on GitHub or any public platform.
{{% /notice %}}

2. Click **Download .csv file** and save the file on your computer for future use.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/021-saveaccesskey.png)

3. Click **Done** to finish.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/022-saveaccesskey.png)

---

#### Step 9: Verify the Access Key

1. Return to the **Security credentials** tab, and you will see the **Access Key ID** listed.
2. Check that the Access Key is in **Active** status.

![Access Key](/images/2.environmentsetup/2.2-createiamuserandattachpolicy/023-checkaccesskey.png)

> 💡 **Note**: Make sure you saved the **Secret Access Key** from the previous step. If not, you’ll need to delete and recreate a new access key.
>
> -   You may **deactivate** or **delete** this key when no longer needed.
> -   Never **commit Access Keys to GitHub** or share them publicly.
> -   If your key is exposed, go to IAM > User > Access Keys > Deactivate then Delete.
