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

![Test get all invoices](/images/5/5.2/001-postman.png)

2. Select **Blank Collection**.

![Test get all invoices](/images/5/5.2/002-newcollection.png)

3. Name it: `InvoiceGetAPI-Tests`

![Test get all invoices](/images/5/5.2/003-changename.png)

---

#### Step 2: Create Request

1. Within the newly created collection, click the **"+"** button to add a request.

![Test get all invoices](/images/5/5.2/004-addrequest.png)

2. Name the request: `Get All Invoices`.

![Test get all invoices](/images/5/5.2/005-getallinvoice.png)

3. Select the **POST** method.

![Test get all invoices](/images/5/5.2/006-choosemethod.png)

5. Go to API Gateway and select the API: `GetInvoiceAPI`.

![Test get all invoices](/images/5/5.2/007-apigateway.png)

6. Navigate to the **Stages** section.

![Test get all invoices](/images/5/5.2/008-apigateway.png)

7. Click the **"+"** button to reveal the `/invoice` endpoint path as shown below:

![Test get all invoices](/images/5/5.2/009.png)

8. Select the **GET** method and copy the **Invoke URL**.

![Test get all invoices](/images/5/5.2/010.png)

9. Paste the **Invoke URL** into Postman as follows:

![Test get all invoices](/images/5/5.2/011.png)

10. Click the **Send** button to view results.

![Test get all invoices](/images/5/5.2/013-send.png)

14. A successful response will appear as follows:

![Test get all invoices](/images/5/5.2/012-result.png)

> Verify that all 3 invoices are returned to confirm success ✅
