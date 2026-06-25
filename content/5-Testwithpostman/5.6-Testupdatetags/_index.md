---
title: "Testing Invoice Category Update"
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

#### Step 1: Create Request

1. In the **InvoiceGetAPI-Tests** Collection, click the **"+"** button to create a new request.

![Update Invoice Tags](/images/5/5.6/Screenshot_1.png)

2. Name the request: `Update Invoice Tags`.

![Update Invoice Tags](/images/5/5.6/Screenshot_2.png)

3. Select the **PATCH** method.

![Update Invoice Tags](/images/5/5.6/Screenshot_3.png)

4. Go to API Gateway and select the API: `GetInvoiceAPI`.

5. Navigate to the **Stages** section.

6. Click the **"+"** button to reveal the `/invoice/tags/{id}` endpoint path as shown below:

![Update Invoice Tags](/images/5/5.6/Screenshot_4.png)

8. Select the **PATCH** method and copy the **Invoke URL**.

![Update Invoice Tags](/images/5/5.6/Screenshot_5.png)

9. Paste the **Invoke URL** into Postman as follows:

![Update Invoice Tags](/images/5/5.6/Screenshot_6.png)

10. Replace `{id}` in the API endpoint with an actual Invoice ID from DynamoDB:

```bash
https://x4uqolxky6.execute-api.us-east-1.amazonaws.com/dev/invoice/tags/<InvoiceId_from_DynamoDB>
```

![Update Invoice Tags](/images/5/5.6/Screenshot_7.png)

11. Go to the **Body** tab → Select **raw** → Choose **JSON**.

![Update Invoice Tags](/images/5/5.6/Screenshot_8.png)

12. Paste the following **JSON** code into Postman to update invoice categories:

```json
{
    "tags": ["VIP", "Urgent"]
}
```

13. Click the **Send** button to view results.

![Update Invoice Tags](/images/5/5.6/Screenshot_11.png)

14. The response will appear as follows:

![Update Invoice Tags](/images/5/5.6/Screenshot_12.png)

15. Check the `Tags` field in **DynamoDB** to verify the update.

![Update Invoice Tags](/images/5/5.6/Screenshot_13.png)
