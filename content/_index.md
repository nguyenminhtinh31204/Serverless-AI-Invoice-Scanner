---
title: "Building an AI Invoice Scanning System on a Serverless Architecture"
weight: 1
chapter: false
---

# Building an AI Invoice Scanning System on a Serverless Architecture

### Overview

In this lab, you will build an AI-powered invoice scanning system using a serverless architecture on AWS. The application allows users to upload invoice files, store them in Amazon S3, extract text and structured information using Amazon Textract, normalize the extracted data with the OpenAI API, store the final invoice records in Amazon DynamoDB, and access the data through Amazon API Gateway.

The frontend is deployed using AWS Amplify Hosting and uses Amazon Cognito for user sign-in and sign-up. The system also uses Amazon CloudWatch to monitor logs and troubleshoot backend processing issues.

{{% notice info %}}
In this project, the AI data normalization step uses the OpenAI API instead of Amazon Bedrock. The OpenAI API key should be stored securely on the backend, such as in Lambda environment variables or AWS Secrets Manager, and should not be exposed in the frontend.
{{% /notice %}}

![Architecture Diagram](/images/architecture-log.png)

---

#### Amazon S3

Amazon S3 (Simple Storage Service) is AWS’s object storage service used to store uploaded invoice files. In this system, invoice files are uploaded to an S3 bucket, usually under the `uploads/` folder. After a file is uploaded, an S3 event can trigger a Lambda function to start the invoice processing workflow.

#### Amazon Textract

Amazon Textract is an AI service that extracts text, tables, forms, and structured data from documents such as invoices. In this system, Textract is used to read invoice content and reduce manual data entry.

#### OpenAI API

The OpenAI API is used to analyze and normalize the text extracted by Amazon Textract. It helps convert raw OCR text into structured invoice data such as customer name, invoice number, invoice date, total amount, currency, and line-item information.

{{% notice warning %}}
The OpenAI API is an external service and is not part of the AWS Cloud. The API key must be stored securely on the backend and should never be placed in React source code or public repositories.
{{% /notice %}}

#### AWS Lambda

AWS Lambda is a serverless compute service that runs backend logic without requiring server management. In this project, Lambda functions are used to handle invoice uploads, process uploaded files, call Amazon Textract and the OpenAI API, and manage invoice data stored in DynamoDB.

The system uses several Lambda functions, such as:

- `UploadInvoiceFileFunction`
- `FetchInvoiceDetailsFunction`

#### Amazon DynamoDB

Amazon DynamoDB is a fully managed NoSQL database used to store invoice data after it has been extracted and normalized. In this system, DynamoDB stores information such as invoice ID, customer name, invoice number, invoice date, total amount, currency, tags, starred status, and extracted invoice details.

#### Amazon API Gateway

Amazon API Gateway is used to create and manage REST API endpoints for the application. In this system, API Gateway exposes endpoints for uploading invoice files, retrieving invoices, searching invoices, and updating invoice metadata such as tags and starred status.

Example API routes include:

```txt
POST /uploads
GET /invoice
GET /invoice/{id}
GET /invoice?name=<customer_name>
PATCH /invoice/tags/{id}
PATCH /invoice/starred/{id}
```

{{% notice info %}}
API Gateway is only protected by Amazon Cognito if a Cognito Authorizer is explicitly configured. If the API routes are not connected to a Cognito Authorizer, Cognito only protects frontend sign-in and sign-up.
{{% /notice %}}

#### Amazon Cognito

Amazon Cognito is used to provide user authentication for the frontend application. It allows users to sign up, sign in, and access the React application securely. The frontend uses Cognito User Pool configuration such as User Pool ID, App Client ID, and AWS Region.

#### AWS Amplify Hosting

AWS Amplify Hosting is used to build and deploy the React frontend application. The frontend is connected to a GitHub repository and uses environment variables to connect to Cognito and API Gateway endpoints.

In this project, Amplify is used for hosting only. Cognito and backend resources are configured separately through the AWS Console.

#### Amazon CloudWatch

Amazon CloudWatch is a monitoring and logging service used to collect logs from Lambda functions and API Gateway. It helps troubleshoot errors related to file upload, invoice processing, Textract extraction, OpenAI API calls, DynamoDB operations, and CORS issues.

---

### Content

1. [Introduction](1-Introduce/)
2. [Environment Setup](2-Environmentsetup/)
3. [AI-Powered Invoice Processing](3-AIpoweredinvoiceprocessing/)
4. [Deploying API Gateway](4-Deployingapigateway/)
5. [Test with Postman](5-Testwithpostman/)
6. [Deploying Frontend](6-Deployingfrontend/)
7. [Resource Cleanup](7-Cleanup/)
