+++
title = "Remove API Gateway"
date = 2025
weight = 1
chapter = false
pre = "<b>7.1 </b>"
+++

#### Overview

In this step, you will delete the **Amazon API Gateway** resources that were created for the **Serverless AI Invoice Scanner** system.

Amazon API Gateway was used to expose REST API endpoints for the frontend application. These APIs allowed users to upload invoice files, retrieve invoice data, search invoices, update tags, and mark important invoices.

After completing the lab, the API Gateway resources should be removed to prevent unused endpoints from remaining active.

{{% notice warning %}}
After deleting an API Gateway API, all routes and stages under that API will be removed. The frontend application will no longer be able to call the deleted API endpoints.
{{% /notice %}}

---

#### Resources to Remove

In this project, the API Gateway resources to remove are:

| API Name | Purpose |
|---|---|
| `PostInvoiceAPI` | Handles invoice upload requests from the frontend. |
| `GetInvoiceAPI` | Handles invoice retrieval, search, tag update, and starred invoice update requests. |

The main API routes used in this project include:

```txt
POST /uploads
GET /invoice
GET /invoice/{id}
GET /invoice?name=<customer_name>
PATCH /invoice/tags/{id}
PATCH /invoice/starred/{id}
```

---

#### Steps to Follow

- Open the **AWS Management Console**.

- Search for and open the **API Gateway** service.

![Remove API Gateway](/images/7/7.1/Screenshot_1.png)

- In the API list, select the API used for uploading invoice files:

```txt
PostInvoiceAPI
```

![Remove API Gateway](/images/7/7.1/Screenshot_2.png)

- Open the **API actions** menu.

- Choose **Delete API**.

![Remove API Gateway](/images/7/7.1/Screenshot_3.png)

- A confirmation dialog will appear.

- Enter the required confirmation text:

```txt
confirm
```

![Remove API Gateway](/images/7/7.1/Screenshot_4.png)

- Choose **Delete** to confirm the deletion.

- After the deletion is completed, the API will no longer appear in the API Gateway list.

![Remove API Gateway](/images/7/7.1/Screenshot_5.png)

---

#### Delete the Remaining API

Repeat the same steps to delete the second API:

```txt
GetInvoiceAPI
```

![Remove API Gateway](/images/7/7.1/Screenshot_6.png)

This API was used for invoice management features such as:

- Viewing all invoices.
- Viewing invoice details.
- Searching invoices by ID or customer name.
- Updating invoice tags.
- Marking invoices as important.

---

#### Verify the Deletion

After deleting both API Gateway APIs:

- Return to the **API Gateway** API list.
- Confirm that `PostInvoiceAPI` and `GetInvoiceAPI` no longer appear.
- Try calling the previous API endpoints from Postman or the frontend application.
- The deleted endpoints should no longer be accessible.

---

#### Impact on the System

After removing API Gateway, the frontend can no longer communicate with the backend Lambda functions through the deleted endpoints.

| Component | Impact |
|---|---|
| React frontend | Cannot upload, search, retrieve, or update invoices. |
| Upload Lambda | No longer receives requests from `POST /uploads`. |
| Invoice Management Lambda | No longer receives invoice query or update requests. |
| Amazon S3 | Existing uploaded files remain unless the S3 bucket is deleted separately. |
| Amazon DynamoDB | Existing invoice data remains unless the table is deleted separately. |
| Amazon Cognito | Not directly affected by API Gateway deletion. |
| Amazon CloudWatch | Existing API Gateway logs may remain unless deleted separately. |

---

#### Important Notes

{{% notice info %}}
Deleting API Gateway does not automatically delete Lambda functions, S3 buckets, DynamoDB tables, Cognito User Pools, or CloudWatch log groups. These resources must be deleted separately in the following cleanup steps.
{{% /notice %}}

{{% notice warning %}}
If the frontend application still contains the deleted API Gateway URLs in its environment variables, API calls will fail. This is expected after cleanup.
{{% /notice %}}

---

#### Conclusion

You have successfully removed the API Gateway resources used in the **Serverless AI Invoice Scanner** system.

- `PostInvoiceAPI` has been deleted.
- `GetInvoiceAPI` has been deleted.
- The frontend can no longer call the backend through these API endpoints.
- This cleanup step helps prevent unused API endpoints from remaining active and reduces the risk of unexpected requests or costs.
