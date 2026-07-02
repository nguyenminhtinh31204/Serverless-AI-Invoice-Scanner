---

title: "Introduction"
weight: 1
chapter: false
pre: " <b> 1. </b> "
--------------------

#### Introduction

In this lab, you will build a **Serverless AI Invoice Scanner** using AWS services and an external AI API. The system allows users to upload invoice files, extract text from documents, normalize invoice information, store the extracted data, and manage invoices through a web-based dashboard.

The solution consists of the following core components:

* **AWS Amplify Hosting** – to host and deploy the React frontend application.
* **Amazon Cognito** – to handle user sign-up, sign-in, and frontend authentication.
* **Amazon API Gateway** – to expose REST APIs for uploading invoices, retrieving invoice data, updating tags, and marking important invoices.
* **AWS Lambda** – to process API requests, upload invoice files, trigger document processing, and manage invoice data.
* **Amazon S3** – to store uploaded invoice files such as PDF, PNG, JPG, and JPEG.
* **Amazon Textract** – to extract text from uploaded invoice documents using OCR.
* **OpenAI API** – to normalize and structure the extracted invoice text into fields such as customer name, invoice number, invoice date, total amount, and currency.
* **Amazon DynamoDB** – to store invoice metadata, extracted data, tags, starred status, and processing results.
* **Amazon CloudWatch** – to monitor Lambda logs, API errors, and system behavior during testing and debugging.

{{% notice info %}}
In this project, Amazon Bedrock was replaced by the OpenAI API for AI-based data normalization. The OpenAI API key is stored on the backend side, such as in Lambda environment variables or AWS Secrets Manager, and is not exposed in the frontend application.
{{% /notice %}}

#### Learning Objectives

By completing this lab, you will gain hands-on experience in:

* Designing and building a serverless invoice processing architecture.
* Hosting a React frontend application using AWS Amplify Hosting.
* Configuring Amazon Cognito for user authentication.
* Creating REST APIs with Amazon API Gateway and AWS Lambda.
* Uploading and storing invoice files in Amazon S3.
* Using Amazon Textract to extract text from invoice documents.
* Integrating the OpenAI API to normalize and structure extracted invoice data.
* Storing and querying invoice data with Amazon DynamoDB.
* Implementing invoice management features such as search, detail view, tags, starred invoices, sorting, filtering, and Excel export.
* Monitoring and troubleshooting backend services using Amazon CloudWatch.
* Understanding CORS, API Gateway routes, Lambda permissions, and environment variables in a serverless application.
