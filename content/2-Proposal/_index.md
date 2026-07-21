---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# LunaGenZ - Serverless Numerology Web Application
## Automated Numerology Application System on AWS Serverless Platform

---

### 1. Project Overview
LunaGenZ is a Numerology application platform built for young people (Gen Z), allowing users to look up personalized numbers based on their birthdate and full name. The system automatically generates detailed PDF reports and sends them directly via email to users.

To ensure flexibility, stability, and cost optimization for the MVP (Minimum Viable Product) phase, the platform applies 100% AWS Serverless architecture and uses powerful internal PDF generation technology instead of relying on third-party Generative AI services.

---

### 2. Problem Statement & Solution

#### Current Problem
The current market has many fortune-telling and numerology applications, but most require users to pay from the start or have designs that are not user-friendly for young customers (Gen Z). Initially, the team planned to use Generative AI (like Amazon Bedrock) to generate personalized reports. However, integrating LLM (Large Language Models) poses uncontrollable cost risks for an MVP project and increases latency when generating reports.

#### Technical Solution (Fallback Strategy)
LunaGenZ decided to pivot to building a robust internal PDF report generation system. This solution uses a fully Serverless Event-Driven architecture:

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Frontend | Next.js + AWS Amplify | User interface |
| API Layer | Amazon API Gateway | Receive HTTP requests |
| Compute | AWS Lambda (Node.js) | Process logic, generate PDF |
| Database | Amazon DynamoDB | Store request metadata |
| Storage | Amazon S3 | Store PDF files |
| Email | Amazon SES | Send automated emails |

---

### 3. Benefits and Return on Investment

The internal PDF generator strategy provides:

- **$0 cost** for third-party AI API calls
- **Fast response time** due to no LLM dependency
- **100% control** over report output format
- **Automatic scaling** when large numbers of users search simultaneously (e.g., when trending on TikTok)
- **Near $0 initial cost** by leveraging AWS Free Tier effectively

---

### 4. Solution Architecture

#### AWS Services Used

| Service | Role |
|---------|------|
| AWS Amplify | Hosting for Next.js web application with automatic CI/CD |
| Amazon API Gateway | Gateway to receive HTTP requests from Frontend |
| AWS Lambda | Run Node.js functions for numerology calculation, PDF rendering |
| Amazon DynamoDB | High-speed NoSQL database, store user search history |
| Amazon S3 | Object Storage for secure PDF report storage |
| Amazon SES | Automated email service with PDF attachment |

#### Workflow

1. Customer enters Full Name and Birthdate on Next.js interface
2. Frontend sends data via Amazon API Gateway
3. AWS Lambda Function receives request, calculates personal numbers
4. Lambda calls PDF generation library to render report
5. Request payload is recorded in Amazon DynamoDB
6. Successfully generated PDF is uploaded to Amazon S3 Bucket
7. Lambda calls Amazon SES to send email with report to user

---

### 5. Budget Estimation

Because the system is 100% Serverless-based, the monthly maintenance budget during the first year (MVP phase) is extremely low thanks to AWS Free Tier:

| Service | Free Tier Limit | Cost |
|---------|----------------|------|
| AWS Lambda | 1 million requests/month | $0 |
| Amazon API Gateway | 1 million REST API calls/month | $0 |
| Amazon DynamoDB | 25GB storage, 25 WCU/RCU | $0 |
| Amazon S3 | 5GB Standard storage | $0 |
| Amazon SES | 62,000 emails/month | $0 |
| AWS Amplify | 1000 build minutes/month, 5GB storage | $0 |

**Estimated Total Cost: ~$0/month** during the first phase (MVP proof of concept)

---

### 6. Risk Assessment

#### Risk 1: Amazon SES Sandbox
- **Issue**: Email credibility of Amazon SES in Sandbox mode is limited
- **Solution**: Submit request to AWS Support to exit Sandbox mode, verify Domain before officially launching to public

#### Risk 2: Lambda Cold Start
- **Issue**: AWS Lambda "Cold Start" problem when no users for long periods, slowing PDF generation
- **Solution**: Optimize Node.js code, use lightweight PDF generation libraries, and set Provisioned Concurrency if needed during scaling phase

---

### 7. Expected Outcomes

- Complete Serverless platform with near $0 operating cost
- System automatically scales with high traffic
- PDF reports generated and emailed automatically within seconds
- Gen Z-friendly interface
