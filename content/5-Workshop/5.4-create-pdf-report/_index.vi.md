---
title: "Tạo báo cáo PDF"
weight: 4
chapter: true
pre: " <b> 5.4. </b> "
---

# Tạo báo cáo PDF

## Tổng quan

Phần này hướng dẫn chúng em tạo Lambda function để tự động tạo báo cáo PDF từ kết quả tính toán thần số học.

## Nội dung

1. **5.4.1 - Tạo Lambda PDF Generator** - Viết Lambda để tạo PDF sử dụng PDFKit

## Luồng xử lý

```
Lambda Calculation → Result → Lambda PDF Generator → Save to S3 → Return URL
```

## Bước tiếp theo

Chuyển sang phần **Tạo Lambda PDF Generator** để triển khai việc tạo PDF.
