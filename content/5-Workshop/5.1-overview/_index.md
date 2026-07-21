---
title: "Overview"
weight: 1
chapter: true
pre: " <b> 5.1. </b> "
---

---

## About LunaGenZ

LunaGenZ is an innovative Numerology application, designed to calculate and generate detailed numerology reports for individuals. To ensure high availability, scalability, and cost optimization, the entire application is built on **AWS Serverless architecture**.

## Architecture Overview

In this workshop, you will deploy the following components:

### 1. Frontend (Amazon S3 & CloudFront)
Static user interface (built with HTML/CSS/JS or Framework). Hosted on S3 and distributed globally via CloudFront.

### 2. Backend (Amazon API Gateway & AWS Lambda)
Core numerology calculation logic processed by AWS Lambda functions, triggered securely through API Gateway.

### 3. PDF Report Generation (AWS Lambda)
A dedicated Serverless function automatically compiles calculation results into downloadable PDF reports.

## What Will You Build?

By the end of this workshop, you will have a complete Serverless application running on AWS, with Frontend interface, Backend logic, and automatic PDF report generation capabilities. Let's get started!

---

## Next Step

Proceed to **Prerequisites** to ensure you have prepared all necessary tools.
