---
title: "Workshop"
date: 2026-01-01
weight: 5
chapter: false
pre: " 5. "
---

# Triển khai LunaGenZ (Thần Số Học) trên AWS Serverless

#### Tổng quan

**LunaGenZ** là một dự án Thần Số Học được xây dựng hoàn toàn trên kiến trúc AWS Serverless. Workshop này sẽ hướng dẫn toàn bộ quy trình triển khai logic Backend, tự động hóa việc tạo báo cáo PDF nội bộ, và host ứng dụng Frontend.

Trong bài lab này, chúng em đã thực hiện:
+ Xây dựng và triển khai Backend Serverless bằng AWS Lambda và API Gateway.
+ Tự động hóa quá trình sinh báo cáo PDF dựa trên các chỉ số Thần Số Học.
+ Host giao diện Frontend đảm bảo tính sẵn sàng cao trên Amazon S3 và Amazon CloudFront.

#### Nội dung

1. [Tổng quan Workshop](5.1-overview/)
2. [Chuẩn bị (Prerequisites)](5.2-prerequisites/)
3. [Triển khai Backend (Lambda & API Gateway)](5.3-backend-lambda/)
4. [Tự động sinh báo cáo PDF](5.4-create-pdf-report/)
5. [Hosting Frontend (S3 & CloudFront)](5.5-hosting-frontend/)
6. [Dọn dẹp tài nguyên (Clean up)](5.6-cleanup/)

#### Sơ đồ kiến trúc

![LunaGenZ Architecture](/images/lunagenz-architecture.svg)

#### Liên kết

- **Website:** [https://www.lunagenz.sbs/](https://www.lunagenz.sbs/)
