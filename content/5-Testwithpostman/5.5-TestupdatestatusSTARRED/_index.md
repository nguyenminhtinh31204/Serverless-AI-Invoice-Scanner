---
title: "Testing Invoice Star Status Update"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Update Invoice Starred](/images/5/5.5/Screenshot_1.png)

2. Name the request: `Update Invoice Starred`.

![Update Invoice Starred](/images/5/5.5/Screenshot_2.png)

3. Select the **PATCH** method.

![Update Invoice Starred](/images/5/5.5/Screenshot_3.png)

4. Go to API Gateway and select the API: `GetInvoiceAPI`.

5. Navigate to the **Stages** section.

6. Click the **"+"** button to reveal the `/invoice/starred/{id}` endpoint path as shown below:

![Update Invoice Starred](/images/5/5.5/Screenshot_4.png)

8. Select the **PATCH** method and copy the **Invoke URL**.

![Update Invoice Starred](/images/5/5.5/Screenshot_5.png)

9. Paste the **Invoke URL** into Postman as follows:

![Update Invoice Starred](/images/5/5.5/Screenshot_6.png)

10. Replace `{id}` in the API endpoint with an actual Invoice ID from DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/starred/<InvoiceId_from_DynamoDB>
```

![Update Invoice Starred](/images/5/5.5/Screenshot_7.png)

11. Go to the **Body** tab → Select **raw** → Choose **JSON**.

![Update Invoice Starred](/images/5/5.5/Screenshot_8.png)

12. Paste the following **JSON** code into Postman to mark the invoice:

```json
{
    "starred": true
}
```

![Update Invoice Starred](/images/5/5.5/Screenshot_9.png)

13. Click the **Send** button to view results.

![Update Invoice Starred](/images/5/5.5/Screenshot_10.png)

14. The response will appear as follows:

![Update Invoice Starred](/images/5/5.5/Screenshot_11.png)

15. Check the `Starred` field in **DynamoDB** to verify the update.

![Update Invoice Starred](/images/5/5.5/Screenshot_12.png)
