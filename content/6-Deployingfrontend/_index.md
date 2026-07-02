---

title: "Frontend Deployment"
weight: 6
chapter: false
pre: " <b> 6 </b> "
-------------------

### Requirements

Before deploying the frontend application, make sure you have prepared the following tools and services:

* Visual Studio Code.
* Node.js and npm.
* Git and a GitHub account.
* An AWS account with permission to use Amazon Cognito and AWS Amplify Hosting.
* The API Gateway endpoints created in the previous steps.
* The React frontend source code.

{{% notice info %}}
In this project, AWS Amplify is used as **Amplify Hosting** to host the React frontend. Amazon Cognito is configured manually from the AWS Console instead of using `amplify init`, `amplify add auth`, and `amplify push`.
{{% /notice %}}

---

#### Step 1: Download and Open the Source Code

* Download the frontend source code.(https://github.com/nguyenminhtinh31204/frontend-invoice-scanner.git)
* Extract the downloaded zip file.
* Open **Visual Studio Code**.
* Open the extracted project folder.

The project structure should contain files and folders similar to the following:

```txt
public/
src/
.env.example
package.json
README.md
```

Open the terminal in Visual Studio Code by pressing:

```txt
Ctrl + ~
```

Then navigate to the project folder:

```bash
cd <folder_name>
```

Install the project dependencies:

```bash
npm install
```

---

#### Step 2: Update the Environment Variables

Open the `.env` file in the project root folder.

If the file does not exist, create a new file named:

```txt
.env
```

Then update the environment variables as follows:

```env
REACT_APP_SEND_AUTH_TOKEN=false

REACT_APP_AWS_REGION=ap-southeast-1

REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx

REACT_APP_API_UPLOAD_URL=https://<PostInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/uploads

REACT_APP_API_INVOICE_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice

REACT_APP_API_UPDATE_TAGS_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/tags

REACT_APP_API_UPDATE_STARRED_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/starred
```

Replace the values with your actual AWS resources:

* **REACT_APP_USER_POOL_ID**: User Pool ID from Amazon Cognito.
* **REACT_APP_USER_POOL_CLIENT_ID**: App Client ID from Amazon Cognito.
* **REACT_APP_API_UPLOAD_URL**: API Gateway endpoint for uploading invoice files.
* **REACT_APP_API_INVOICE_URL**: API Gateway endpoint for retrieving invoice data.
* **REACT_APP_API_UPDATE_TAGS_URL**: API Gateway endpoint for updating invoice tags.
* **REACT_APP_API_UPDATE_STARRED_URL**: API Gateway endpoint for marking invoices as important.

{{% notice warning %}}
Do not store the OpenAI API key in the React `.env` file. The OpenAI API key must be stored on the backend side, such as in Lambda environment variables or AWS Secrets Manager.
{{% /notice %}}

---

#### Step 3: Configure Amazon Cognito

Because AWS Amplify Gen 1 CLI is no longer used in this deployment process, Amazon Cognito is created manually from the AWS Console.

Go to:

```txt
AWS Console → Amazon Cognito → User pools → Create user pool
```

Configure the user pool as follows:

* **Application type**: Single-page application (SPA).
* **Application name**: `invoice-scanner-frontend`.
* **Sign-in identifiers**: Email.
* **Self-registration**: Enable self-registration.
* **Required attributes**: Email.
* **User pool name**: `invoice-scanner-user-pool`.
* **App client name**: `invoice-scanner-frontend-client`.
* **Client secret**: Do not generate a client secret.

For local development, add the following URL:

```txt
http://localhost:3000/
```

to:

* Allowed callback URLs.
* Allowed sign-out URLs.

After creating the user pool, copy the following values:

```txt
User Pool ID
App Client ID
```

Then paste them into the `.env` file:

```env
REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

---

#### Step 4: Configure Cognito in React

Create a file named:

```txt
src/awsConfig.js
```

Add the following configuration:

```javascript
const awsConfig = {
  Auth: {
    Cognito: {
      userPoolId: process.env.REACT_APP_USER_POOL_ID,
      userPoolClientId: process.env.REACT_APP_USER_POOL_CLIENT_ID,
      loginWith: {
        email: true
      }
    }
  }
};

export default awsConfig;
```

Install the required Amplify libraries:

```bash
npm install aws-amplify @aws-amplify/ui-react
```

In `src/App.js`, the application uses `Authenticator` from Amplify UI to display the login and sign-up screen before allowing users to access the invoice dashboard.

---

#### Step 5: Run the Application Locally

Start the React application locally:

```bash
npm start
```

The application will run at:

```txt
http://localhost:3000
```

When the application opens, the Cognito login screen will appear first.

You can register a new user or sign in with an existing user account.

After signing in, the invoice dashboard will be displayed.

Test the following functions:

* Upload an invoice file.
* Drag and drop an invoice file.
* View all invoices.
* View invoice details.
* Search invoices by Invoice ID.
* Search invoices by customer name.
* Export invoices to Excel.
* Filter invoices by date.
* Sort invoices by total amount.
* Sort invoices by invoice date.
* View invoice tags.
* Add invoice tags.
* Edit invoice tags.
* Filter invoices by tags.
* Mark important invoices.

{{% notice info %}}
The `npm start` command is only used for local development. For production deployment, the frontend will be built and hosted by AWS Amplify Hosting.
{{% /notice %}}

---

#### Step 6: Push the Source Code to GitHub

To deploy the frontend without running the local server continuously, push the React project to GitHub.

Initialize Git in the project folder:

```bash
git init
```

Add all project files:

```bash
git add .
```

Commit the source code:

```bash
git commit -m "Deploy invoice scanner frontend"
```

Set the main branch:

```bash
git branch -M main
```

Add your GitHub repository:

```bash
git remote add origin <your_github_repository_url>
```

Push the project to GitHub:

```bash
git push -u origin main
```

---

#### Step 7: Deploy the Frontend with AWS Amplify Hosting

Go to:

```txt
AWS Console → AWS Amplify → Create new app → Host web app
```

Choose:

```txt
GitHub
```

Then select:

* Repository: your frontend repository.
* Branch: `main`.

Amplify will detect the React project automatically. If you need to configure the build settings manually, use the following build specification:

```yaml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - npm install
    build:
      commands:
        - npm run build
  artifacts:
    baseDirectory: build
    files:
      - "**/*"
  cache:
    paths:
      - node_modules/**/*
```

Then choose:

```txt
Save and deploy
```

After deployment completes, Amplify will provide a public URL similar to:

```txt
https://main.xxxxx.amplifyapp.com
```

This is the deployed frontend URL.

---

#### Step 8: Add Environment Variables in Amplify Hosting

In the Amplify Console, open your deployed application.

Go to:

```txt
App settings → Environment variables
```

Add the same environment variables from the local `.env` file:

```env
REACT_APP_SEND_AUTH_TOKEN=false

REACT_APP_AWS_REGION=ap-southeast-1

REACT_APP_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
REACT_APP_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx

REACT_APP_API_UPLOAD_URL=https://<PostInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/uploads

REACT_APP_API_INVOICE_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice

REACT_APP_API_UPDATE_TAGS_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/tags

REACT_APP_API_UPDATE_STARRED_URL=https://<GetInvoiceAPI_ID>.execute-api.ap-southeast-1.amazonaws.com/dev/invoice/starred
```

After adding or editing environment variables, redeploy the application.

{{% notice warning %}}
React environment variables are injected during build time. If you update environment variables in Amplify Hosting, you must redeploy the application for the new values to take effect.
{{% /notice %}}

---

#### Step 9: Update Cognito Callback and Sign-out URLs

After Amplify Hosting finishes deployment, copy the deployed frontend URL.

Example:

```txt
https://main.xxxxx.amplifyapp.com/
```

Go back to Amazon Cognito:

```txt
Amazon Cognito → User pools → invoice-scanner-user-pool → App client
```

Add the Amplify Hosting URL to:

```txt
Allowed callback URLs
```

and:

```txt
Allowed sign-out URLs
```

For example:

```txt
http://localhost:3000/
https://main.xxxxx.amplifyapp.com/
```

This allows Cognito authentication to work both locally and on the deployed Amplify Hosting website.

---

#### Step 10: Test the Deployed Frontend

Open the deployed Amplify Hosting URL:

```txt
https://main.xxxxx.amplifyapp.com
```

Sign in with Cognito.

Then test all main features again:

* Upload an invoice file.
* Drag and drop an invoice file.
* View invoice details.
* View all invoices.
* Search invoices by ID.
* Search invoices by customer name.
* Export invoice data to Excel.
* Filter invoices by invoice date.
* Sort invoices by total amount.
* Sort invoices by invoice date.
* View invoice tags.
* Add invoice tags.
* Edit invoice tags.
* Filter invoices by tags.
* Mark important invoices.

---

### Troubleshooting

#### Upload failed

If invoice upload fails, check the following:

* `REACT_APP_API_UPLOAD_URL` must point to the correct API Gateway endpoint.
* API Gateway must have the route:

```txt
POST /uploads
```

* Lambda upload function must return CORS headers.
* API Gateway must support the `OPTIONS` method for CORS preflight.
* The request body must contain:

```json
{
  "filename": "invoice.png",
  "file": "base64_string_here",
  "contentType": "image/png"
}
```

#### Missing Authentication Token

If API Gateway returns:

```json
{
  "message": "Missing Authentication Token"
}
```

Check:

* The HTTP method is correct.
* The route path is correct.
* The API Gateway stage is correct.
* The API was redeployed after route changes.

#### Cognito login works locally but not on Amplify Hosting

Check:

* The Amplify Hosting URL was added to Cognito callback URLs.
* The Amplify Hosting URL was added to Cognito sign-out URLs.
* The environment variables were added in Amplify Hosting.
* The app was redeployed after environment variables were updated.

#### API works in Postman but fails in browser

This is usually caused by CORS.

Make sure Lambda or API Gateway returns the following headers:

```json
{
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Headers": "Content-Type,Authorization",
  "Access-Control-Allow-Methods": "OPTIONS,GET,POST,PATCH"
}
```

---

### Result

After completing this step, the frontend is deployed on AWS Amplify Hosting.

Users can access the invoice scanner from a public URL without running `npm start` locally.

The frontend connects to:

* Amazon Cognito for authentication.
* Amazon API Gateway for REST APIs.
* AWS Lambda for backend processing.
* Amazon S3 for invoice file storage.
* Amazon Textract and OpenAI API for text extraction and data structuring.
* Amazon DynamoDB for storing and querying invoice data.
