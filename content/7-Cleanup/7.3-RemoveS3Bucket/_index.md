+++
title = "Remove S3 Bucket"
date = 2025
weight = 3
chapter = false
pre = "<b>7.3 </b>"
+++

#### Overview

In this step, you will delete the **Amazon S3 Bucket** that was created to store uploaded invoice files for the **Serverless AI Invoice Scanner** system.

Amazon S3 was used to store invoice files uploaded from the React frontend. These files were usually stored in the following folder:

```txt
uploads/
```

After completing the lab, you should delete the S3 bucket and all objects inside it to avoid keeping unused storage resources and to reduce unnecessary costs.

{{% notice warning %}}
When an S3 bucket is deleted, all invoice files inside the bucket will be permanently removed if they are not backed up first. Make sure you no longer need these files before continuing.
{{% /notice %}}

---

#### Resource to Remove

In this project, the S3 bucket was used to store uploaded invoice files.

| Component | Description |
|---|---|
| S3 Bucket | Stores invoice files uploaded from the frontend. |
| `uploads/` folder | Contains uploaded invoice files such as PDF, PNG, JPG, or JPEG files. |
| S3 Event Notification | Triggers the processing Lambda function after a new invoice file is uploaded. |
| Object metadata | May include information such as content type, file name, or upload time. |

The bucket name may be similar to:

```txt
invoice-scanner-upload-bucket
```

or the bucket name you selected during the deployment process.

{{% notice info %}}
S3 bucket names are globally unique. Therefore, the actual bucket name in your AWS account may be different from the example used in this document.
{{% /notice %}}

---

#### Check the S3 Event Notification Before Deleting

Before deleting the bucket, you should check whether the bucket has an event notification configured.

In this system, the S3 bucket may trigger the processing Lambda function:

```txt
S3 Bucket → Processing Lambda
```

This event notification was used to automatically start invoice processing after a file was uploaded.

If the processing Lambda function has already been deleted in the previous cleanup step, this event notification is no longer needed.

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **S3** service.

![Remove S3 Bucket](/images/7/7.3/Screenshot_1.png)

- In the bucket list, select the S3 bucket used for the **Serverless AI Invoice Scanner** project.

Example:

```txt
invoice-scanner-upload-bucket
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_2.png)

- Open the bucket and review the objects inside it.

The uploaded invoice files are usually stored in:

```txt
uploads/
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_3.png)

---

#### Empty the Bucket

Before deleting the bucket, you must remove all objects inside it.

- Select all objects or folders inside the bucket.

- Choose **Delete**.

![Remove S3 Bucket](/images/7/7.3/Screenshot_4.png)

- A confirmation dialog will appear.

- Enter the required confirmation text. In most cases, AWS asks you to type:

```txt
permanently delete
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_5.png)

- Choose **Delete objects** to delete all selected objects.

{{% notice info %}}
An S3 bucket can only be deleted when it is empty. If the bucket still contains objects, AWS will not allow the bucket to be deleted.
{{% /notice %}}

---

#### Delete the S3 Bucket

After the bucket is empty:

- Return to the Amazon S3 bucket list.

- Select the bucket that you want to delete.

![Remove S3 Bucket](/images/7/7.3/Screenshot_7.png)

- Choose **Delete**.

![Remove S3 Bucket](/images/7/7.3/Screenshot_8.png)

- Enter the bucket name to confirm the deletion.

Example:

```txt
invoice-scanner-upload-bucket
```

![Remove S3 Bucket](/images/7/7.3/Screenshot_9.png)

- Choose **Delete bucket** to complete the deletion.

![Remove S3 Bucket](/images/7/7.3/Screenshot_10.png)

---

#### If Bucket Versioning Is Enabled

If **S3 Versioning** was enabled on the bucket, deleting current objects may not fully empty the bucket.

In this case, you must also delete:

- Object versions.
- Delete markers.

To do this:

- Open the bucket.
- Enable **Show versions**.
- Select all object versions and delete markers.
- Choose **Delete**.
- After the bucket is completely empty, return to the bucket list and delete the bucket.

{{% notice warning %}}
If S3 Versioning is enabled, the bucket may still fail to delete even after you remove the current objects. Always check object versions and delete markers before deleting the bucket.
{{% /notice %}}

---

#### Verify the Deletion

After deleting the S3 bucket:

- Return to the Amazon S3 bucket list.
- Confirm that the project bucket no longer appears.
- Try uploading an invoice from the frontend.
- The upload function should no longer work because the storage bucket has been removed.
- If old S3 event notifications still exist, review Lambda or S3 event configurations to make sure there are no unused references.

---

#### Impact on the System

After the S3 bucket is deleted, the system no longer has a storage location for uploaded invoice files.

| Component | Impact |
|---|---|
| React frontend | Cannot successfully upload invoice files. |
| Upload Lambda | Cannot store files in the deleted S3 bucket. |
| Processing Lambda | No longer receives S3 events for new uploads. |
| Amazon Textract | No longer receives new invoice files from S3 for text extraction. |
| OpenAI API | No longer receives newly extracted invoice text for normalization. |
| Amazon DynamoDB | Existing invoice data remains if the table has not been deleted. |
| Amazon CloudWatch | Existing logs remain if Log Groups have not been deleted. |

---

#### Important Notes

{{% notice info %}}
Deleting the S3 bucket does not automatically delete invoice records stored in DynamoDB. If processed invoice data has already been saved to DynamoDB, you must delete the DynamoDB table in a separate cleanup step.
{{% /notice %}}

{{% notice warning %}}
If a Lambda function still exists and its environment variables point to the deleted S3 bucket, future Lambda invocations may fail because the bucket no longer exists.
{{% /notice %}}

{{% notice info %}}
If the bucket had lifecycle rules, event notifications, or bucket policies, those configurations are removed together with the bucket.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the Amazon S3 bucket used in the **Serverless AI Invoice Scanner** system.

- The invoice files inside the `uploads/` folder have been deleted.
- The S3 bucket used for invoice file storage has been removed.
- The S3 event notification to the processing Lambda function is no longer active.
- The system can no longer store new invoice files in Amazon S3.
- This cleanup step helps remove unused storage resources and avoid unnecessary storage costs.
