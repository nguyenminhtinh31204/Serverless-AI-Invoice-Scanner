---
title: "Testing Invoice File Upload"
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

#### Prerequisites

-   Postman installed ([https://www.postman.com/downloads](https://www.postman.com/downloads)).

---

#### Resource Preparation

Download the following files before testing the API in Postman:

-   [demo_invoice.png](https://drive.google.com/uc?export=download&id=1p_sRXNN9saZLPYJNATZNqV-NmydsUo3L)
-   [demo_invoice2.png](https://drive.google.com/uc?export=download&id=1D-5nvOvdM2Fo2hBTDICTgE8xY-kg7vvY)
-   [demo_invoice3.png](https://drive.google.com/uc?export=download&id=1h_4q-1Xy-MSuwAqXW7D4oTTsnjdn0USh)

---

#### Step 1: Convert Images to Base64

We will use the online tool [base64-image.de](https://www.base64-image.de/):

1. Visit the website and select the file **demo_invoice.png**.

![Website base64-image.de](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/001-websitebase64.png)

2. The website will automatically convert it to Base64.

![Convert to Base64](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/002-convert-to-base64.png)

3. Click **</> show code** to retrieve the Base64 string.

![Click show code](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/003-click-show-code.png)

4. Copy the Base64 code.

![Copy Base64](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/004-copy-base64.png)

5. Save it temporarily in Notepad.

6. Repeat the process for the remaining two files.

---

#### Step 2: Create a Postman Collection

1. Open the Postman application.

![Postman](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/005-postman.png)

2. Click the **"+"** button to create a new collection.

![Create new collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/006-create-new-collection.png)

3. Select **Blank Collection**.

![Blank collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/007-blank-collection.png)

4. Name it: `InvoiceUploadAPI-Tests`

![Rename collection](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/008-rename-collection.png)

---

#### Step 3: Create a Request

1. Inside the newly created collection, click the **"+"** button to add a request.

![Create request](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/009-create-request.png)

2. Name the request: `Upload Invoice`.

![Rename request](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/010-rename-request.png)

3. Select the **POST** method.

![Choose method post](/images/4.deployingapigatewayandfrontend/4.3-testwithpostman/011-choose-method-post.png)

5. Go to API Gateway, select the API: `PostInvoiceAPI`.

![PostInvoiceAPI](/images/4.deployingapigatewayandfrontend/4.4/image.png)

6. Navigate to the **Stages** section.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(1).png>)

7. Click the **“+”** button to reveal the full URL.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(2).png>)

8. Select the **POST** method and copy the **Invoke URL**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(3).png>)

9. Paste the **Invoke URL** into Postman as shown:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(4).png>)

10. Go to the **Headers** tab and configure as follows:

```bash
Content-Type: application/json
```

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(5).png>)

11. Navigate to the **Body** tab → Select **raw** → Choose **JSON**.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(6).png>)

12. Paste the following **JSON** into Postman:

```json
{
    "file": "",
    "filename": ""
}
```

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(7).png>)

13. Paste the Base64 string saved earlier in Notepad and specify the filename as follows:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(8).png>)

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(9).png>)

14. Click the **Send** button to view the results.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(10).png>)

15. A successful response will appear as follows:

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(11).png>)

16. Go to **S3** → Navigate to the `uploads/` folder → Verify the uploaded file.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(12).png>)

17. Access **DynamoDB** → Select **Explore items** → Choose the **InvoiceData** table.

![PostInvoiceAPI](</images/4.deployingapigatewayandfrontend/4.4/image(13).png>)
