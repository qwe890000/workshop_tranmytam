---
title: "Overview"
weight: 1
chapter: true
pre: " <b> 5.1. </b> "
---

# Overview

## About LunaGenZ

LunaGenZ is an innovative Numerology application designed to calculate and generate detailed personal numerology reports. To ensure high availability, scalability, and cost optimization, the entire application is built on **AWS Serverless architecture**.

## Architecture Overview

In this workshop, you will deploy the following components:

### 1. Frontend (Amazon S3 & CloudFront)
Static user interface (built with HTML/CSS/JS or Framework). It is stored on S3 and distributed globally via CloudFront.

### 2. Backend (Amazon API Gateway & AWS Lambda)
Core numerology calculation logic is processed by AWS Lambda functions, triggered securely through API Gateway.

### 3. PDF Report Generation (AWS Lambda)
A dedicated Serverless function that automatically compiles calculation results into a downloadable PDF report.

## Architecture Diagram

![LunaGenZ Architecture](/images/lunagenz-architecture.svg)

## Workflow

1. **User** accesses website via **CloudFront**
2. **CloudFront** distributes content from **S3 Bucket**
3. User enters information (Full Name, Birthdate)
4. **Frontend** sends request to **API Gateway**
5. **API Gateway** triggers **Lambda Function**
6. **Lambda** calculates and returns results
7. When needed, **Lambda PDF Generator** creates PDF report

## What Will You Build?

By the end of this workshop, you will have a complete Serverless application running on AWS, with Frontend interface, Backend logic, and automatic PDF report generation capability. Let's get started!

---

## Next Steps

Proceed to **Prerequisites** section to begin.
