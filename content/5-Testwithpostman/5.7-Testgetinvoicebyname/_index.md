---
title: "Testing Invoice Retrieval by Customer Name"
weight: 7
chapter: false
pre: " <b> 5.7 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Get Invoices By Name](/images/5/5.7/001.png)

2. Name the request: `Get Invoices By Name`.

![Get Invoices By Name](/images/5/5.7/002.png)

3. Select the **GET** method.

![Get Invoices By Name](/images/5/5.7/003.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

6. Navigate to the **Stages** section.

7. Click the **"+"** button to reveal the `/invoice` endpoint path as shown below:

![Get Invoices By Name](/images/5/5.7/004.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Get Invoices By Name](/images/5/5.7/005.png)

9. Paste the **Invoke URL** into Postman as follows:

![Get Invoices By Name](/images/5/5.7/006.png)

10. Append the following query parameter to the API endpoint:

```bash
?name=<customer_name>
```

11. Enter the customer name you want to search for in the invoices.

![Get Invoices By Name](/images/5/5.7/007.png)

10. Click the **Send** button to view results.

![Get Invoices By Name](/images/5/5.7/008.png)

11. The response will appear as follows:

![Get Invoices By Name](/images/5/5.7/009.png)
