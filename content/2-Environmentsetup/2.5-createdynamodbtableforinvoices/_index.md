---
title: "Create DynamoDB Table"
weight: 5
chapter: false
pre: " <b> 2.5 </b> "
---

#### Overview

In this step, you will create a **DynamoDB Table** to store invoice information after it is processed by the Lambda function. The table will use **on-demand** mode and have 2 additional **Global Secondary Indexes (GSIs)** to support queries by customer name and starred invoices.

---

#### Step 1: Access DynamoDB Console

1. Log in to [AWS Console](https://console.aws.amazon.com/), search for **DynamoDB**, then select **DynamoDB** in the results.

![Open DynamoDB](/images/2.environmentsetup/2.5-createdynamodb/001-opendynamodb.png)

{{% notice info %}}
💡 **Note:** Before clicking **Create table**, make sure you have selected the correct **region as US East (N. Virginia) (us-east-1)** in the upper right corner of the AWS Console screen.

{{% /notice %}}

2. Click **Create table** to start creating a new table.

![Create Table](/images/2.environmentsetup/2.5-createdynamodb/002-createtable.png)

---

#### Step 2: Configure DynamoDB table

1. **Table name**: `InvoiceData`

2. **Partition key**:

-   **Name**: `InvoiceId`
-   **Type**: `String`

3. Skip the **Sort key** section (not necessary).

![Table Key](/images/2.environmentsetup/2.5-createdynamodb/003-tablekeys.png)

4. In the **Table settings** section, Select **Default settings** to switch to **On-demand capacity** mode.

![Billing Mode](/images/2.environmentsetup/2.5-createdynamodb/004-ondemand.png)

5. Click **Create table** to finish.

![Create Table](/images/2.environmentsetup/2.5-createdynamodb/005-finishcreate.png)

---

#### Step 3: Add Global Secondary Index (GSI)

Once the **InvoiceData** table is successfully created, you will add two secondary indexes:

---

##### GSI #1: CustomerName-index

1. In the table details page, select the **Indexes** tab.

![Create Index](/images/2.environmentsetup/2.5-createdynamodb/006-indexes.png)

2. Click **Create index**.

![Create Index](/images/2.environmentsetup/2.5-createynamodb/007-createindex.png)

3. Configuration:

-   **Partition key**: `CustomerName`
-   **Data type**: String
-   **Sort key**: _(leave blank)_
-   **Projected attributes**: select **All**

![GSI 1](/images/2.environmentsetup/2.5-createynamodb/008-gsi1.png)

![GSI 1](/images/2.environmentsetup/2.5-createynamodb/009-gsi1.png)

4. Click **Create index**.

![GSI 1](/images/2.environmentsetup/2.5-createdynamodb/010-gsi1.png)

---

##### GSI #2: StarredInvoicesIndex

1. Click **Create index** again to create the second GSI.

![GSI 2](/images/2.environmentsetup/2.5-createynamodb/011-gsi2.png)

2. Configuration:

-   **Partition key**: `IsStarred` (type **String**)
-   **Sort key**: `CreatedAt` (type **String**)
-   **Index name**: `StarredInvoicesIndex`
-   **Projected attributes**: select **All**

![GSI 2](/images/2.environmentsetup/2.5-createynamodb/012-gsi2.png)

![GSI 2](/images/2.environmentsetup/2.5-createynamodb/013-gsi2.png)

3. Click **Create index**.

![GSI 2](/images/2.environmentsetup/2.5-createdynamodb/014-gsi2.png)

4. Make sure both GSIs **CustomerName-index** and **StarredInvoicesIndex** are in **ACTIVE** status before continuing with Lambda function configuration.

![Check GSI Status](/images/2.environmentsetup/2.5-createdynamodb/015-gsistatus.png)

{{% notice warning %}}
⚠️ If the GSI status is still **Creating**, you need to wait a few minutes until it changes to **Active** before making a query or deploying a Lambda to access the GSI.
{{% /notice %}}
