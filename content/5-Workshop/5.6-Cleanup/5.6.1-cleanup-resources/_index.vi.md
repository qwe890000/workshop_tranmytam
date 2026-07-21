---
title: "Dọn dẹp Resources"
weight: 6
chapter: true
pre: " <b> 5.6.1. </b> "
---

# Dọn dẹp Resources

## Giới thiệu

Để tránh phát sinh chi phí không mong muốn trên tài khoản AWS, việc xóa các resources đã tạo trong workshop này rất quan trọng.

## Các bước dọn dẹp

### 1. Xóa S3 Buckets

- Empty và xóa bucket `lunagenz-reports-bucket`
- Empty và xóa bucket `lunagenz-frontend-web`

### 2. Xóa CloudFront Distribution

- Truy cập **CloudFront** trong AWS Console
- Chọn distribution của bạn
- Chọn **Disable** (có thể mất vài phút)
- Sau khi **Disabled**, chọn và click **Delete**

### 3. Xóa API Gateway

- Truy cập **API Gateway** trong AWS Console
- Chọn `LunaGenZ-API` của bạn
- Chọn **Actions** → **Delete**

### 4. Xóa Lambda Functions

- Truy cập **Lambda** trong AWS Console
- Xóa các functions sau:
  - `LunaGenZ-Calculation`
  - `LunaGenZ-PDFGenerator`

### 5. Xóa Lambda Layers (nếu có)

- Truy cập **Lambda** → **Additional resources** → **Layers**
- Xóa các layers bạn đã tạo

### 6. Xóa IAM Roles và Policies

- Truy cập **IAM** trong AWS Console
- Xóa bất kỳ custom Roles hoặc Policies nào được tạo riêng cho workshop này

## Xác nhận đã dọn dẹp

Kiểm tra AWS Console để đảm bảo:
- ✅ Không còn Lambda functions
- ✅ Không còn API Gateways
- ✅ S3 buckets đã empty và xóa
- ✅ CloudFront distributions đã disable và xóa
- ✅ Không còn custom IAM roles

## Hoàn thành

Chúc mừng bạn đã hoàn thành workshop **Triển khai LunaGenZ trên AWS Serverless**!

---

## Hoàn thành

Chúc mừng bạn đã hoàn thành workshop **Triển khai LunaGenZ trên AWS Serverless**!
