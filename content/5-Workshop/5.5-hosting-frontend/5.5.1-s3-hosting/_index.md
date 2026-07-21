---
title: "S3 Hosting"
weight: 5
chapter: true
pre: " <b> 5.5.1. </b> "
---

---

## Introduction

This section guides you through hosting the LunaGenZ frontend on Amazon S3 with static website hosting enabled.

## Step 1: Create S3 Bucket

### Access S3 Console

1. AWS Console → Find **S3**
2. Press **Create bucket**

### Configure Bucket

| Setting | Value |
|---------|-------|
| **Bucket name** | `lunagenz-frontend` |
| **AWS Region** | ap-southeast-1 |
| **Block Public Access** | ❌ Uncheck (for static hosting) |

### Enable Static Website Hosting

1. Select bucket → **Properties**
2. Find **Static website hosting**
3. Press **Edit** → **Enable**

| Setting | Value |
|---------|-------|
| **Hosting type** | Host a static website |
| **Index document** | `index.html` |
| **Error document** | `error.html` |

### Configure Bucket Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::lunagenz-frontend/*"
        }
    ]
}
```

## Step 2: Upload Files

### Upload Frontend Files

1. Select bucket → **Objects**
2. Press **Upload**
3. Drag and drop all frontend files:
   - `index.html`
   - `styles.css`
   - `app.js`
   - `images/` folder

## Summary

After this step, you have:

- ✅ S3 bucket configured for static hosting
- ✅ Bucket policy allowing public access
- ✅ Frontend files uploaded

## Next Steps

Proceed to **CloudFront** section to distribute content globally with HTTPS.
