---
title: "Tổng quan"
weight: 1
chapter: true
pre: " <b> 5.1. </b> "
---

# Tổng quan

## Về LunaGenZ

LunaGenZ là ứng dụng Thần Số Học sáng tạo, được thiết kế để tính toán và tạo báo cáo thần số học chi tiết cho cá nhân. Để đảm bảo tính khả dụng cao, khả năng mở rộng và tối ưu chi phí, toàn bộ ứng dụng được xây dựng trên **kiến trúc Serverless của AWS**.

## Tổng quan Kiến trúc

Trong workshop này, bạn sẽ triển khai các thành phần sau:

### 1. Frontend (Amazon S3 & CloudFront)
Giao diện người dùng tĩnh (được xây dựng bằng HTML/CSS/JS hoặc Framework). Được lưu trữ trên S3 và phân phối toàn cầu qua CloudFront.

### 2. Backend (Amazon API Gateway & AWS Lambda)
Logic tính toán thần số học cốt lõi được xử lý bởi các hàm AWS Lambda, được kích hoạt một cách bảo mật thông qua API Gateway.

### 3. Tạo báo cáo PDF (AWS Lambda)
Một hàm Serverless chuyên dụng tự động tổng hợp kết quả tính toán thành báo cáo PDF có thể tải xuống.

## Bạn sẽ xây dựng gì?

Cuối workshop này, bạn sẽ có một ứng dụng Serverless hoàn chỉnh chạy trên AWS, với giao diện Frontend, logic Backend, và khả năng tạo báo cáo PDF tự động. Hãy bắt đầu!

---

## Bước tiếp theo

Chuyển sang **Điều kiện tiên quyết** để đảm bảo đã chuẩn bị đầy đủ các công cụ cần thiết.
