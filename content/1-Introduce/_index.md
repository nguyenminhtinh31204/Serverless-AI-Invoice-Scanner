---
title: "Introduction"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

#### Introduction

In this lab, you’ll build a **Serverless AI Invoice Scanner** using AWS services. The solution consists of the following core components:

-   **Amazon S3** – to store uploaded invoice files.
-   **AWS Lambda** – to process invoices, orchestrating Amazon Textract and Bedrock.
-   **Amazon DynamoDB** – to store the extracted and normalized data.
-   **Amazon API Gateway** – to expose secure APIs for uploading and retrieving invoice data.
-   **AWS Amplify** – to host and manage the frontend application.
-   **Amazon Cognito** – to handle user authentication and control API access.
-   **Amazon CloudWatch** – to monitor logs, metrics, and overall system health.

#### Learning Objectives

By completing this lab, you’ll gain hands-on experience in:

-   Designing and building a serverless AI-driven architecture.
-   Using Amazon Textract and Bedrock to extract and enrich invoice data.
-   Creating secure, scalable APIs with API Gateway, Lambda, and DynamoDB.
-   Deploying a modern frontend with AWS Amplify and securing it with Cognito.
-   Monitoring performance, optimizing costs, and cleaning up AWS resources.
