---
title: "CloudFront CDN"
weight: 7
chapter: true
pre: " <b> 5.5.2. </b> "
---

# CloudFront CDN

## Giới thiệu

CloudFront giúp phân phối nội dung toàn cầu với tốc độ cao và độ trễ thấp.

## Tạo CloudFront Distribution

### Bước 1: Truy cập CloudFront
- Mở **CloudFront** trong AWS Console
- Chọn **Create distribution**

### Bước 2: Cấu hình Origin
- **Origin domain**: Chọn S3 bucket `lunagenz-frontend-web.s3.amazonaws.com`
- **Origin access**: Chọn **Legacy access identities** (để cho phép CloudFront truy cập S3 bucket private)

### Bước 3: Cấu hình Default Cache Behavior
- **Viewer protocol policy**: Redirect HTTP to HTTPS
- **Allowed HTTP methods**: GET, HEAD
- **Cache key and origin requests**: Cache policy recommended

### Bước 4: Cấu hình Distribution Settings
- **Price class**: Use only North America and Europe (hoặc All edge locations)
- **Alternate domain name (CNAME)**: `workshop.example.com` (tùy chọn)
- **SSL certificate**: Request hoặc chọn certificate đã có
- **Default root object**: `index.html`

### Bước 5: Tạo Distribution
- Chọn **Create distribution**
- Đợi vài phút để status chuyển sang **Deployed**

## Cập nhật S3 Bucket Policy cho CloudFront

Sau khi tạo CloudFront, cập nhật Bucket Policy để chỉ cho phép CloudFront truy cập:

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

## Kiểm tra

Truy cập website qua CloudFront URL: `https://d12345678.cloudfront.net`

---

## Bước tiếp theo

Chuyển sang **Dọn dẹp Resources** để xóa tất cả resources đã tạo.
