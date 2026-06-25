---
title: "Enable Nova Pro"
weight: 6
chapter: false
pre: " <b> 2.6 </b> "
---

#### Overview

In this step, you will enable the **Nova Pro** model on Amazon Bedrock for use in the Serverless Invoices Scanner project. This is one of Amazon's powerful language models, suitable for tasks such as extracting, analyzing, and classifying information from invoices.

{{% notice tip %}}
You only need to activate the model once per region. For this project, make sure you are operating in **us-east-1 (N. Virginia)** to stay in sync with Lambda, S3, and other services.
{{% /notice %}}

#### Step 1: Access Amazon Bedrock Console

1. Sign in to the [AWS Console](https://console.aws.amazon.com/), search for **Bedrock**, then select **Amazon Bedrock**.

![Open Bedrock](/images/2.environmentsetup/2.6-enablebedrock/001-openbedrock.png)

2. In the Amazon Bedrock interface, on the left navigation panel, choose **Model access**.

![Model Access](/images/2.environmentsetup/2.6-enablebedrock/002-modelaccess.png)

---

#### Step 2: Enable the Nova Pro model

1. In the **Base models** section, scroll to find the row named **Nova Pro**.

2. Click **Available to request**, then select **Request model access**.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/003-enablenovapro.png)

3. Click **Next** to continue.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/004-enablenovapro.png)

4. Click **Submit** to send your request for model access.

![Enable Nova Pro](/images/2.environmentsetup/2.6-enablebedrock/005-enablenovapro.png)

5. Once successfully submitted, wait a few seconds to a few minutes. The model status will change to **Access granted**.

![Enable Access](/images/2.environmentsetup/2.6-enablebedrock/006-accessgranted.png)

---

#### Completion

You have successfully enabled the Nova Pro model on Amazon Bedrock. This model is now ready to be used in the Lambda Function for processing invoice data in the Serverless Invoices Scanner system.
