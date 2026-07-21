---
title: "Clean up Resources"
weight: 6
chapter: true
pre: " <b> 5.6.1. </b> "
---

# Clean up Resources

## Introduction

To avoid unexpected charges on your AWS account, it is very important to delete the resources created in this workshop.

## Clean up Steps

### 1. Delete S3 Buckets

- Empty and delete bucket `lunagenz-reports-bucket`
- Empty and delete bucket `lunagenz-frontend-web`

### 2. Delete CloudFront Distribution

- Access **CloudFront** in AWS Console
- Select your distribution
- Select **Disable** (this may take a few minutes)
- After **Disabled**, select and click **Delete**

### 3. Delete API Gateway

- Access **API Gateway** in AWS Console
- Select your `LunaGenZ-API`
- Select **Actions** → **Delete**

### 4. Delete Lambda Functions

- Access **Lambda** in AWS Console
- Delete the following functions:
  - `LunaGenZ-Calculation`
  - `LunaGenZ-PDFGenerator`

### 5. Delete Lambda Layers (if any)

- Access **Lambda** → **Additional resources** → **Layers**
- Delete the layers you created

### 6. Delete IAM Roles and Policies

- Access **IAM** in AWS Console
- Delete any custom Roles or Policies created specifically for this workshop

## Verify Cleanup

Check AWS Console to ensure:
- ✅ No Lambda functions remaining
- ✅ No API Gateways remaining
- ✅ S3 buckets emptied and deleted
- ✅ CloudFront distributions disabled and deleted
- ✅ No custom IAM roles remaining

## Completion

Congratulations on completing the **Deploy LunaGenZ on AWS Serverless** workshop!
