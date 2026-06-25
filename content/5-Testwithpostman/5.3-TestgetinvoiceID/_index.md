---
title: "Testing Invoice Retrieval by ID"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Test get all invoices by ID](/images/5/5.3/Screenshot_1.png)

2. Name the request: `Get Invoices By ID`.

![Test get all invoices by ID](/images/5/5.3/Screenshot_2.png)

3. Select the **GET** method.

![Test get all invoices by ID](/images/5/5.3/Screenshot_3.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

6. Navigate to the **Stages** section.

7. Click the **"+"** button to reveal the `/invoice/{id}` endpoint path as shown below:

![Test get all invoices by ID](/images/5/5.3/Screenshot_4.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Test get all invoices by ID](/images/5/5.3/Screenshot_5.png)

9. Paste the **Invoke URL** into Postman as follows:

![Test get all invoices by ID](/images/5/5.3/Screenshot_6.png)

10. Replace `{id}` in the API endpoint with an actual Invoice ID from DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/<InvoiceId_from_DynamoDB>
```

![Test get all invoices by ID](/images/5/5.3/Screenshot_7.png)

![Test get all invoices by ID](/images/5/5.3/Screenshot_8.png)

11. Click the **Send** button to view results.

![Test get all invoices by ID](/images/5/5.3/Screenshot_9.png)

14. A successful response will appear as follows:

![Test get all invoices by ID](/images/5/5.3/Screenshot_10.png)

> You can now test the remaining 2 invoice files!
