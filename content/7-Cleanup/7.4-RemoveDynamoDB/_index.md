+++
title = "Delete DynamoDB Table"
date = 2025
weight = 4
chapter = false
pre = "<b>7.4 </b>"
+++

#### Overview

In this step, you will delete the **Amazon DynamoDB Table** that was created to store invoice data in the **Serverless AI Invoice Scanner** system.

Amazon DynamoDB is used to store invoice information after the file is processed by Lambda, Amazon Textract, and the OpenAI API. Data in the table may include customer information, invoice code, invoice date, total amount, currency, tags, and important markup status.

After completing the lab, you should delete the DynamoDB table to avoid retaining test data and prevent unnecessary costs.

{{% notice warning %}}
When deleting a DynamoDB Table, all data within the table will be permanently deleted. Please ensure you have backed up the necessary data before performing this operation.

{{% /notice %}}

---

#### Resources to delete

In this project, the DynamoDB Table to be deleted is:

```txt
InvoiceData
```

This table is used to store extracted and normalized invoice data.

Some data commonly stored in the table includes:

| Data Fields | Description |
|---|---|
| `InvoiceId` | Unique identifier of the invoice. |
| `CustomerName` | Name of the customer or supplier extracted from the invoice. |
| `InvoiceNumber` | Invoice number. |
| `InvoiceDate` | Invoice date. |
| `TotalAmount` | Total amount on the invoice. |
| `Currency` | Currency type, e.g., `USD`, `VND`, `EUR`. |
| `Tags` | List of labels added to the invoice. |
| `Starred` | Status indicating an important invoice. |
| `ExtractedData` | Data extracted and normalized from the invoice content. |

{{% notice info %}}
The table name in your AWS account may be different if you named it differently during deployment. Please check that Lambda uses the correct table in your environment variables or backend source code.

{{% /notice %}}

---

#### Check data before deleting

Before deleting a table, you can quickly check the data in the table.

- Open the **AWS Management Console**.

- Search for and access the **DynamoDB** service.

- Select **Tables**.

- Select the table:

```txt
InvoiceData
```

- Select **Explore table items** to see the saved items.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_1.png)

If you need to retain data for reporting or testing, export or capture the data before deleting.

---

#### Steps to follow

- Open the **AWS Management Console**.

- Search for and access the **DynamoDB** service.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_2.png)

- In the left navigation bar, select **Tables**.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_3.png)

- In the list of tables, select the table used for the project:

```txt
InvoiceData
```

![Remove DynamoDB Table](/images/7/7.4/Screenshot_4.png)

- After opening the table, select **Actions**.

- Select **Delete table**.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_5.png)

- A confirmation dialog will appear.

- Carefully read the confirmation message to ensure you are deleting the correct table.

- Enter the confirmation message as requested by the AWS Console, if any.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_6.png)

- Select **Delete table** to confirm deleting the table.

![Remove DynamoDB Table](/images/7/7.4/Screenshot_7.png)

---
#### Deleting Global Secondary Indexes

If the `InvoiceData` table has additional **Global Secondary Indexes**, these indexes will be deleted along with the table.

In this project, the table might use indexes such as:

```txt
CustomerName-index
**StarredInvoicesIndex**
```

These indexes are used to support:

- Searching for invoices by customer name.

- Filtering the list of invoices marked as important.

{{% notice info %}}

You don't need to delete Global Secondary Indexes separately if you delete the entire DynamoDB table. When the table is deleted, the indexes belonging to that table are also deleted.

{{% /notice %}}

---

#### Checking After Deletion

After deleting the DynamoDB table:

- Return to the **Tables** list in DynamoDB.

- Verify that the `InvoiceData` table no longer appears.

- If the table is in a `Deleting` state, wait a few minutes until the deletion process is complete.

- Try calling the APIs to retrieve the invoice list or search for invoices again.

- APIs related to invoice data will no longer return data because the table has been deleted.

---

#### System Impact

After deleting a DynamoDB Table, all invoice data in the system will be lost.

| Component | Impact |
|---|---|
| React frontend | No longer able to display the list, details, or search results of invoices. |
| Invoice Management Lambda | Cannot query or update invoice data. |
| Processing Lambda | Cannot save new invoice processing results to DynamoDB. |
| API Gateway | The API may still exist, but requests related to invoice data will fail. |
| Amazon S3 | Invoice files in S3 are not affected if the bucket has not been deleted. | | Amazon Texttract | Not directly affected. |
| OpenAI API | Not directly affected. |
| CloudWatch Logs | Old logs remain if Log Groups are not deleted. |

---
