---
title: "Deploy Frontend to S3"
weight: 5
chapter: true
pre: " <b> 5.5.1. </b> "
---

# Deploy Frontend to S3

## Introduction

This section guides you through deploying the Frontend interface on Amazon S3.

## Create S3 Bucket

### Step 1: Create New Bucket
- Access **S3** in AWS Console
- Select **Create bucket**
- Bucket name: `lunagenz-frontend-web`
- Region: `Asia Pacific (Singapore) ap-southeast-1`
- Select **Create bucket**

### Step 2: Enable Static Website Hosting
- Select the bucket you just created
- Go to **Properties** tab
- Scroll down to **Static website hosting**
- Select **Enable**
- Index document: `index.html`
- Error document: `error.html`
- Select **Save changes**

### Step 3: Upload Files
- Go to **Objects** tab
- Select **Upload**
- Drag and drop or select files:
  - `index.html`
  - `styles.css`
  - `app.js`
  - Other asset files

### Step 4: Configure Bucket Policy
- Go to **Permissions** tab
- Select **Edit** in **Block public access**
- Uncheck **Block all public access** → **Save changes**
- Add Bucket Policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::lunagenz-frontend-web/*"
        }
    ]
}
```

### Step 5: Test
Access the endpoint: `http://lunagenz-frontend-web.s3-website.ap-southeast-1.amazonaws.com`

---

## Next Steps

Proceed to **CloudFront CDN** to distribute content globally.
