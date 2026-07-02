---
title: "Testing Invoice Retrieval by ID"
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Test get all invoices by ID](/images/5/5.3/001.png)

2. Name the request: `Get Invoices By ID`.

![Test get all invoices by ID](/images/5/5.3/002.png)

3. Select the **GET** method.

![Test get all invoices by ID](/images/5/5.3/003.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

6. Navigate to the **Stages** section.

7. Click the **"+"** button to reveal the `/invoice/{id}` endpoint path as shown below:

![Test get all invoices by ID](/images/5/5.3/004.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Test get all invoices by ID](/images/5/5.3/005.png)

9. Paste the **Invoke URL** into Postman as follows:

![Test get all invoices by ID](/images/5/5.3/006.png)

10. Replace `{id}` in the API endpoint with an actual Invoice ID from DynamoDB:

```bash
https://pwxyscvv7i.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/<InvoiceId_from_DynamoDB>
```

![Test get all invoices by ID](/images/5/5.3/007.png)

![Test get all invoices by ID](/images/5/5.3/008.png)

11. Click the **Send** button to view results.

![Test get all invoices by ID](/images/5/5.3/009-send.png)

14. A successful response will appear as follows:

![Test get all invoices by ID](/images/5/5.3/010-result.png)

> You can now test the remaining 2 invoice files!
