---
title: "Testing Retrieval of All Invoices"
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

#### Prerequisites

> ⚠️ Previously we only demonstrated uploading "demo_invoice.png". Please **upload the remaining 2 invoice files** as well!

---

#### Step 1: Create Postman Collection

1. Open Postman application and click the **"+"** button to create a new collection.

![Test get all invoices](/images/5/5.2/Screenshot_1.png)

2. Select **Blank Collection**.

![Test get all invoices](/images/5/5.2/Screenshot_2.png)

3. Name it: `InvoiceGetAPI-Tests`

![Test get all invoices](/images/5/5.2/Screenshot_3.png)

---

#### Step 2: Create Request

1. Within the newly created collection, click the **"+"** button to add a request.

![Test get all invoices](/images/5/5.2/Screenshot_4.png)

2. Name the request: `Get All Invoices`.

![Test get all invoices](/images/5/5.2/Screenshot_5.png)

3. Select the **POST** method.

![Test get all invoices](/images/5/5.2/Screenshot_6.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

![Test get all invoices](/images/5/5.2/Screenshot_7.png)

6. Navigate to the **Stages** section.

![Test get all invoices](/images/5/5.2/Screenshot_8.png)

7. Click the **"+"** button to reveal the `/invoice` endpoint path as shown below:

![Test get all invoices](/images/5/5.2/Screenshot_9.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Test get all invoices](/images/5/5.2/Screenshot_10.png)

9. Paste the **Invoke URL** into Postman as follows:

![Test get all invoices](/images/5/5.2/Screenshot_11.png)

10. Click the **Send** button to view results.

![Test get all invoices](/images/5/5.2/Screenshot_12.png)

14. A successful response will appear as follows:

![Test get all invoices](/images/5/5.2/Screenshot_15.png)

> Verify that all 3 invoices are returned to confirm success ✅
