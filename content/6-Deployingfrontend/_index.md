---
title: "Frontend Deployment"
weight: 6
chapter: false
pre: " <b> 6 </b> "
---

### Requirements

-   Ensure you have downloaded Visual Studio Code. If not, download it from the following link: [https://code.visualstudio.com/download](https://code.visualstudio.com/download).

---

#### Step 1: Download the Source Code

-   Click [Download the Source Code](https://drive.google.com/uc?export=download&id=1pbOd59G4p2Z0K4T2nTDBZAqTRO1Ghksi).
-   Extract the downloaded zip file.

#### Step 2: Update API Endpoint

-   Open **Visual Studio Code**.
-   Open the `.env` file and replace the API endpoint values with the corresponding API IDs from API Gateway:
    -   **REACT_APP_API_UPLOAD_URL** : Use the **API ID** of `PostInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_INVOICE_URL** : Use the **API ID** of `GetInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_UPDATE_TAGS_URL** : Use the **API ID** of `GetInvoiceAPI` (AWS API Gateway).
    -   **REACT_APP_API_UPDATE_STARRED_URL** : Use the **API ID** of `GetInvoiceAPI` (AWS API Gateway).

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_1.png)

#### Step 3: Configure Amplify

-   Open **Terminal** by pressing `Ctrl + ~`.

-   Navigate to the project's root directory using:

```bash
cd <folder_name>
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_2.png)

-   Run the following command to **install dependencies**:

```bash
npm install
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_3.png)

-   Run the following command to install **AWS Amplify CLI**:

```bash
npm install -g @aws-amplify/cli
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_4.png)

-   After installation, verify that **AWS Amplify CLI** is installed successfully by running:

```bash
amplify -v
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_5.png)

-   Use the following command to configure Amplify:

```bash
amplify configure
```

{{% notice warning %}}
⚠️ **Note**: After running the `amplify configure` command, a new browser tab will open. You can close the tab and return to **Visual Studio Code** to continue the configuration.
{{% /notice %}}

-   In the Amplify configuration, proceed as follows:

    -   **Region**: Select `us-east-1`.
    -   **Access Key ID**: Enter the **access key** of the IAM User you created.
    -   **Secret Access Key**: Enter the **secret key** of the IAM User you created.
    -   **Profile Name**: Select `default`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_6.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_7.png)

-   Run the following command to **initialize Amplify**:

```bash
amplify init
```

-   Configure the initialization as follows:

    -   **? Do you want to use an existing environment**: Select `No`.
    -   **? Enter a name for the environment**: `dev`.
    -   **? Select the authentication method you want to use**: `AWS access keys`.
    -   **? accessKeyId**: Enter the **access key** of the IAM User you created.
    -   **? secretAccessKey**: Enter the **secret key** of the IAM User you created.
    -   **? region**: `us-east-1`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_8.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_9.png)

#### Step 4: Configure Cognito

-   Run the following command to add **Auth** using **AWS Cognito**:

```bash
amplify add auth
```

-   Configure Auth as follows:

    -   **Do you want to use the default authentication and security configuration?**: `Default configuration`.
    -   **How do you want users to be able to sign in?**: `Email`.
    -   **Do you want to configure advanced settings?**: `No, I am done`.

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_10.png)

-   Then deploy the changes:

```bash
amplify push
```

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_11.png)

![Source code React with AWS Amplify](/images/6/6.1/Screenshot_12.png)

#### Step 5: Run the Application

-   Start the application by running:

```bash
npm start
```

-   Test the following functionalities:

    -   Upload an invoice file.
    -   View invoice details.
    -   View all invoices.
    -   Search invoices by ID or customer name.
    -   Export to Excel.
    -   Filter by date.
    -   Sort by total amount.
    -   Sort by invoice date.
    -   Drag-and-drop for invoice upload.
    -   View invoice tags.
    -   Add invoice tags.
    -   Edit invoice tags.
    -   Filter by tags.
    -   Mark important invoices.
