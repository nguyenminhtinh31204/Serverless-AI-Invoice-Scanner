---
title: "Create S3 Bucket"
weight: 4
chapter: false
pre: " <b> 2.4 </b> "
---

#### Overview

In this step, you will create an **S3 bucket** to store invoice files uploaded by users. This bucket will be created in the **us-east-1** region, and you will also configure an **event notification** to trigger a **Lambda function** whenever a new file is uploaded into a specific folder.

---

#### Step 1: Access S3 Console

1. Log in to the [AWS Console](https://console.aws.amazon.com/), search for **S3**, then select **S3** from the results.

![S3 Console](/images/2.environmentsetup/2.4-creates3bucket/001-opens3console.png)

{{% notice info %}}
💡 **Note:** Before creating the bucket, make sure you have selected the correct region **us-east-1** in the upper right corner of the AWS Console screen.  
Creating the S3 bucket in the correct region is crucial for services like Lambda or Textract to work seamlessly.
{{% /notice %}}

2. Click **Create bucket** to start creating a new one.

![Create Bucket](/images/2.environmentsetup/2.4-creates3bucket/002-createbucket.png)

---

#### Step 2: Configure the bucket

1. **Bucket name**: `invoice-upload-s3-bucket`

![Configure Bucket](/images/2.environmentsetup/2.4-creates3bucket/003-bucketname-region.png)

{{% notice info %}}
💡 **Note:** Bucket names must be globally unique. You may add a suffix if the name is already taken, e.g., **invoice-upload-s3-bucket-123456**.
{{% /notice %}}

2. In the **Object Ownership** section, select **ACLs disabled**.

3. **Block Public Access settings**: keep default settings (all enabled) to ensure data is not public.

![Block Public Access](/images/2.environmentsetup/2.4-creates3bucket/004-publicaccess.png)

4. In the **Bucket Versioning** section, select **Disabled**.

![Versioning](/images/2.environmentsetup/2.4-creates3bucket/006-versioning.png)

5. In the **Default encryption** section, choose **Amazon S3 managed keys (SSE-S3)**.

6. Click **Create bucket** to finish.

![Encryption](/images/2.environmentsetup/2.4-creates3bucket/005-encryption.png)

7. Verify that the **Bucket** has been created.

![Bucket](/images/2.environmentsetup/2.4-creates3bucket/007-check-bucket.png)

---

#### Step 3: Create the `uploads/` folder

1. Inside the newly created bucket, click **Create folder**.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/008-create-folder.png)

2. Name the folder: `uploads`

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/009-folder-name.png)

3. Click **Create folder** to confirm.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/010-createuploads.png)

4. Verify that the folder has been created.

![Create Folder](/images/2.environmentsetup/2.4-creates3bucket/011-checkfolder.png)

---

#### Step 7: Set up Event Notification

1. Go to the **Properties** tab of the bucket.

![Event Notifications](/images/2.environmentsetup/2.4-creates3bucket/012-properties.png)

2. Scroll down to **Event notifications** → select **Create event notification**.

![Event Notifications](/images/2.environmentsetup/2.4-creates3bucket/013-createevent.png)

3. Configure as follows:

    - **Name**: `TriggerLambdaOnUpload`
    - **Prefix**: `uploads/`
    - **Suffix**: _(leave blank)_
    - **Event types**: select `PUT` (All object create events)
    - **Destination**: select **Lambda function**
    - **Lambda function**: select `UploadInvoiceFileFunction`

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/014-eventsettings.png)

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/015-eventsettings.png)

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/016-eventsettings.png)

4. Click **Save changes** to complete the setup.

![Event Settings](/images/2.environmentsetup/2.4-creates3bucket/017-savechanges.png)

{{% notice info %}}
If you don’t see the Lambda function listed, make sure it was created in the correct region **(us-east-1)** and that the Lambda's IAM role has the **s3:PutBucketNotification** permission.
{{% /notice %}}
