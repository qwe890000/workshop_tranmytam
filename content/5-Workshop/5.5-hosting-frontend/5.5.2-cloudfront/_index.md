---
title: "CloudFront CDN"
weight: 6
chapter: true
pre: " <b> 5.5.2. </b> "
---

---

## Introduction

This section guides you through setting up Amazon CloudFront to distribute LunaGenZ frontend globally with HTTPS support.

## Step 1: Create CloudFront Distribution

### Access CloudFront Console

1. AWS Console → Find **CloudFront**
2. Press **Create distribution**

### Configure Distribution

| Setting | Value |
|---------|-------|
| **Origin domain** | `lunagenz-frontend.s3.ap-southeast-1.amazonaws.com` |
| **Origin type** | S3 |
| **Viewer protocol policy** | Redirect HTTP to HTTPS |
| **Compress objects automatically** | ✅ Yes |
| **Price class** | Use all edge locations |

### Default Cache Behavior Settings

| Setting | Value |
|---------|-------|
| **Viewer protocol policy** | Redirect HTTP to HTTPS |
| **Allowed HTTP methods** | GET, HEAD, OPTIONS |
| **Cache key and origin requests** | Cache policy and origin request policy |

## Step 2: Configure Error Pages (Optional)

1. Distribution → **Error pages**
2. Press **Create custom error response**

| Setting | Value |
|---------|-------|
| **HTTP error code** | 403 Forbidden |
| **Customize error response** | ✅ Yes |
| **Response page path** | `/error.html` |
| **HTTP response code** | 200 OK |

## Step 3: Get Distribution URL

After creating distribution:

```
https://xxxxxxxx.cloudfront.net
```

## Summary

After this step, you have:

- ✅ CloudFront distribution for global content delivery
- ✅ HTTPS enabled
- ✅ Caching and optimization configured

## Next Steps

Proceed to **Clean up** section to clean up resources and avoid unnecessary costs.
