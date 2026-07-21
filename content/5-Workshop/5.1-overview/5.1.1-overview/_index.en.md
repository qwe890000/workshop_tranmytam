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

## What Will You Build?

By the end of this workshop, you will have a complete Serverless application running on AWS, with Frontend interface, Backend logic, and automatic PDF report generation capability. Let's get started!

---

## Next Steps

Proceed to **Prerequisites** to ensure you have all required tools.
