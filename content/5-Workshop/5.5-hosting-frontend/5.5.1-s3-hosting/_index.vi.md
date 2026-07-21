---
title: "S3 Hosting"
weight: 5
chapter: true
pre: " <b> 5.5.1. </b> "
---

# Triển khai Frontend lên S3

## Giới thiệu

Phần này hướng dẫn bạn triển khai giao diện Frontend trên Amazon S3.

## Tạo S3 Bucket

### Bước 1: Tạo Bucket mới
- Truy cập **S3** trong AWS Console
- Chọn **Create bucket**
- Bucket name: `lunagenz-frontend-web`
- Region: `Asia Pacific (Singapore) ap-southeast-1`
- Chọn **Create bucket**

### Bước 2: Bật Static Website Hosting
- Chọn bucket vừa tạo
- Vào tab **Properties**
- Cuộn xuống **Static website hosting**
- Chọn **Enable**
- Index document: `index.html`
- Error document: `error.html`
- Chọn **Save changes**

### Bước 3: Upload Files
- Vào tab **Objects**
- Chọn **Upload**
- Kéo thả hoặc chọn files:
  - `index.html`
  - `styles.css`
  - `app.js`
  - Các file asset khác

### Bước 4: Cấu hình Bucket Policy
- Vào tab **Permissions**
- Chọn **Edit** trong **Block public access**
- Bỏ check **Block all public access** → **Save changes**
- Thêm Bucket Policy:

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

### Bước 5: Kiểm tra
Truy cập endpoint: `http://lunagenz-frontend-web.s3-website.ap-southeast-1.amazonaws.com`

---

## Bước tiếp theo

Chuyển sang **CloudFront CDN** để phân phối nội dung toàn cầu.
