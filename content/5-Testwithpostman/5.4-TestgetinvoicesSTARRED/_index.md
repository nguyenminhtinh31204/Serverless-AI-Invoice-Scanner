---
title: "Testing Retrieval of Starred Invoices"
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Get All Invoices Starred](/images/5/5.4/001.png)

2. Name the request: `Get All Invoices Starred`.

![Get All Invoices Starred](/images/5/5.4/002.png)

3. Select the **GET** method.

![Get All Invoices Starred](/images/5/5.4/003.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

6. Navigate to the **Stages** section.

7. Click the **"+"** button to reveal the `/invoice/starred` endpoint path as shown below:

![Get All Invoices Starred](/images/5/5.4/004.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Get All Invoices Starred](/images/5/5.4/005.png)

9. Paste the **Invoke URL** into Postman as follows:

![Get All Invoices Starred](/images/5/5.4/006.png)

10. Click the **Send** button to view results.

![Get All Invoices Starred](/images/5/5.4/007.png)

11. The response will appear as follows:

![Get All Invoices Starred](/images/5/5.4/008.png)

> When the response shows `[]`, it means there are currently no starred invoices
