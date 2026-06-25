---
title: "Building an AI Invoice Scanning System on a Serverless Architecture"
weight: 1
chapter: false
---

# Building an AI Invoice Scanning System on a Serverless Architecture

### Overview

In this lab, you will build an AI-powered invoice scanning system using a Serverless architecture on AWS. The application allows users to upload invoices, extract data using Amazon Textract, analyze and normalize the data with Amazon Bedrock, store it in Amazon DynamoDB, and access it through API Gateway.

The frontend is deployed on AWS Amplify, supporting authentication and authorization with Amazon Cognito. The system is monitored using CloudWatch.

![ConnectPrivate](/images/arc-logg.png)

#### Amazon S3

Amazon S3 (Simple Storage Service) is AWS’s object storage service used to store invoice files and related documents. It ensures high data durability, flexible scalability, and fast access from anywhere.

#### Amazon Textract

Amazon Textract is an AI service for extracting text, tables, and structured data from invoices. Textract eliminates manual data entry and speeds up the digitization process.

#### Amazon Bedrock

Amazon Bedrock is a serverless AI model service used to analyze, interpret, and normalize invoice data. It enables advanced information processing without managing AI infrastructure.

#### AWS Lambda

AWS Lambda is a serverless computing service that runs code to handle events such as invoice uploads, data extraction, and storage without the need to manage servers.

#### Amazon DynamoDB

Amazon DynamoDB is a fully managed NoSQL database used to store invoice data after it has been extracted and analyzed. DynamoDB offers fast performance, automatic scaling, and high reliability.

#### Amazon API Gateway

Amazon API Gateway is a service for creating, managing, and securing APIs. In this system, it provides APIs for uploading invoice files and retrieving stored data.

#### Amazon Cognito

Amazon Cognito is a user authentication and authorization service. It provides secure sign-in for the frontend application and integrates seamlessly with API Gateway and other AWS services.

#### AWS Amplify

AWS Amplify is a frontend development and deployment platform that helps build modern user interfaces and connect directly to backend APIs. The application’s frontend is deployed using Amplify.

#### Amazon CloudWatch

Amazon CloudWatch is a monitoring service that collects and analyzes logs, metrics, and events from all system components. It helps monitor performance and provides early alerts for potential issues.

### Content

1. [Introduction](1-Introduce/)
2. [Environment Setup](2-Environmentsetup/)
3. [AI-Powered Invoice Processing](3-AIpoweredinvoiceprocessing/)
4. [Deploying API Gateway](4-Deployingapigateway/)
5. [Test with Postman](5-Testwithpostman/)
6. [Deploying Frontend](6-Deployingfrontend/)
7. [End-to-End Hands-on](7-Endtoendhandson/)
8. [Resource Cleanup](8-Cleanup/)
