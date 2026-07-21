---
title: "Cleanup Resources"
weight: 6
chapter: true
pre: " <b> 5.6.1. </b> "
---

---

## Introduction

This section guides you through cleaning up all AWS resources created during the workshop to avoid unnecessary charges.

## Cleanup Order

Follow this order to ensure all dependencies are properly removed:

```
CloudFront → API Gateway → Lambda → S3 Buckets → IAM Roles
```

## Step 1: Delete CloudFront Distribution

1. CloudFront Console → Select distribution
2. Press **Disable**
3. Wait for deployment to complete
4. Press **Delete**

## Step 2: Delete API Gateway

1. API Gateway Console → Select API
2. Press **Delete**

## Step 3: Delete Lambda Functions

1. Lambda Console → Select functions
2. Press **Actions** → **Delete**
   - `LunaGenZ-Calculation`
   - `LunaGenZ-PDFGenerator`

## Step 4: Delete S3 Buckets

1. S3 Console → Select buckets
2. Empty bucket first (delete all objects)
3. Then delete the bucket:
   - `lunagenz-frontend`
   - `lunagenz-reports-bucket`

## Step 5: Delete IAM Roles

1. IAM Console → Select roles
2. Delete roles created for Lambda:
   - Roles with `LunaGenZ-*` prefix

## Summary

After this step, you have:

- ✅ All AWS resources cleaned up
- ✅ No unnecessary charges will be incurred

## Congratulations!

You have successfully completed the LunaGenZ Workshop on AWS Serverless!

### What You Built

- **Frontend**: Static website hosted on S3 with CloudFront CDN
- **Backend**: Serverless API using Lambda and API Gateway
- **PDF Generation**: Automatic report generation with PDFKit
- **Architecture**: Fully serverless, auto-scaling, cost-optimized

### Useful Links

- **Workshop Repository**: [GitHub Link]
- **LunaGenZ Website**: [https://www.lunagenz.sbs/](https://www.lunagenz.sbs/)
