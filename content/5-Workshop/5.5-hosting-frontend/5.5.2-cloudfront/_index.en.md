---
title: "CloudFront CDN"
weight: 7
chapter: true
pre: " <b> 5.5.2. </b> "
---

# CloudFront CDN

## Introduction

CloudFront helps distribute content globally with high speed and low latency.

## Create CloudFront Distribution

### Step 1: Access CloudFront
- Open **CloudFront** in AWS Console
- Select **Create distribution**

### Step 2: Configure Origin
- **Origin domain**: Select S3 bucket `lunagenz-frontend-web.s3.amazonaws.com`
- **Origin access**: Select **Legacy access identities** (to allow CloudFront to access private S3 bucket)

### Step 3: Configure Default Cache Behavior
- **Viewer protocol policy**: Redirect HTTP to HTTPS
- **Allowed HTTP methods**: GET, HEAD
- **Cache key and origin requests**: Cache policy recommended

### Step 4: Configure Distribution Settings
- **Price class**: Use only North America and Europe (or All edge locations)
- **Alternate domain name (CNAME)**: `workshop.example.com` (optional)
- **SSL certificate**: Request or select existing certificate
- **Default root object**: `index.html`

### Step 5: Create Distribution
- Select **Create distribution**
- Wait a few minutes for status to change to **Deployed**

## Update S3 Bucket Policy for CloudFront

After creating CloudFront, update Bucket Policy to only allow CloudFront access:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontAccess",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::lunagenz-frontend-web/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::ACCOUNT_ID:distribution/DISTRIBUTION_ID"
                }
            }
        }
    ]
}
```

## Test

Access website via CloudFront URL: `https://d12345678.cloudfront.net`

---

## Next Steps

Proceed to **Clean up Resources** to remove all created resources.
