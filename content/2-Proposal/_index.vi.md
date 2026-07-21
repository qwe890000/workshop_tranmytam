---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
# LunaGenZ - Serverless Numerology Web Application
## Hệ thống Ứng dụng Thần Số Học tự động hoá trên nền tảng AWS Serverless

---

### 1. Tổng quan dự án (Overview)
LunaGenZ là nền tảng ứng dụng Thần Số Học (Numerology) được xây dựng dành cho giới trẻ, cho phép người dùng tra cứu các chỉ số cá nhân hóa dựa trên ngày sinh và họ tên. Hệ thống tự động tạo ra báo cáo chi tiết dưới dạng PDF và gửi trực tiếp qua email cho người dùng.

Để đảm bảo tính linh hoạt, sự ổn định và tối ưu chi phí cho giai đoạn MVP (Minimum Viable Product), nền tảng áp dụng kiến trúc 100% AWS Serverless và sử dụng công nghệ tạo PDF nội bộ mạnh mẽ thay vì phụ thuộc vào các dịch vụ Generative AI của bên thứ ba.

---

### 2. Vấn đề cần giải quyết & Giải pháp

#### Vấn đề hiện tại
Thị trường hiện nay có rất nhiều ứng dụng bói toán, thần số học, nhưng đa số yêu cầu người dùng trả phí ngay từ đầu hoặc thiết kế không thân thiện với tập khách hàng trẻ (Gen Z). Ban đầu, nhóm dự định sử dụng Generative AI (như Amazon Bedrock) để tạo ra các báo cáo cá nhân hóa. Tuy nhiên, việc tích hợp LLM (Large Language Models) tiềm ẩn rủi ro về mặt chi phí không thể kiểm soát đối với một dự án MVP và làm tăng độ trễ (latency) khi sinh báo cáo.

#### Giải pháp kỹ thuật (Fallback Strategy)
LunaGenZ quyết định chuyển hướng (Pivot) sang xây dựng một hệ thống tạo báo cáo nội bộ (Internal PDF Generation) vững chắc. Giải pháp này sử dụng kiến trúc Event-Driven hoàn toàn Serverless:

| Thành phần | Công nghệ | Mục đích |
|------------|-----------|----------|
| Frontend | Next.js + AWS Amplify | Giao diện người dùng |
| API Layer | Amazon API Gateway | Tiếp nhận HTTP requests |
| Compute | AWS Lambda (Node.js) | Xử lý logic, tạo PDF |
| Database | Amazon DynamoDB | Lưu trữ request metadata |
| Storage | Amazon S3 | Lưu trữ file PDF |
| Email | Amazon SES | Gửi email tự động |

---

### 3. Lợi ích và Hoàn vốn đầu tư

Chiến lược sử dụng trình tạo PDF nội bộ mang lại:

- **Chi phí bằng $0** cho việc call API AI bên thứ ba
- **Thời gian phản hồi nhanh** do không phụ thuộc LLM
- **Kiểm soát 100%** định dạng báo cáo đầu ra
- **Tự động scale** khi có lượng lớn người dùng tra cứu cùng lúc (ví dụ khi có trend trên TikTok)
- **Chi phí ban đầu gần như bằng $0** nhờ tận dụng tốt AWS Free Tier

---

### 4. Kiến trúc giải pháp

#### Các Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---------|---------|
| AWS Amplify | Hosting cho ứng dụng web Next.js với tính năng CI/CD tự động |
| Amazon API Gateway | Cửa ngõ tiếp nhận HTTP requests từ Frontend |
| AWS Lambda | Chạy các hàm xử lý logic (Node.js) tính toán thần số học, render PDF |
| Amazon DynamoDB | Cơ sở dữ liệu NoSQL tốc độ cao, lưu trữ lịch sử tra cứu |
| Amazon S3 | Object Storage lưu trữ an toàn các file báo cáo PDF |
| Amazon SES | Dịch vụ gửi thư điện tử tự động đính kèm file PDF |

#### Luồng hoạt động (Workflow)

1. Khách hàng nhập Họ tên và Ngày sinh trên giao diện Next.js
2. Frontend gọi API đẩy dữ liệu qua Amazon API Gateway
3. AWS Lambda Function tiếp nhận request, tính toán các chỉ số cá nhân
4. Lambda gọi thư viện tạo PDF để render báo cáo
5. Payload của request được ghi nhận lại trong Amazon DynamoDB
6. File PDF sau khi tạo thành công được đẩy trực tiếp lên Amazon S3 Bucket
7. Lambda gọi Amazon SES để gửi email chứa báo cáo cho người dùng

---

### 5. Ước tính ngân sách & Chi phí

Vì hệ thống được cấu trúc 100% dựa trên Serverless, ngân sách duy trì hàng tháng trong năm đầu (giai đoạn MVP) là cực kì thấp nhờ tận dụng AWS Free Tier:

| Dịch vụ | Giới hạn Free Tier | Chi phí |
|---------|-------------------|---------|
| AWS Lambda | 1 triệu requests/tháng | $0 |
| Amazon API Gateway | 1 triệu REST API calls/tháng | $0 |
| Amazon DynamoDB | 25GB lưu trữ, 25 WCU/RCU | $0 |
| Amazon S3 | 5GB Standard storage | $0 |
| Amazon SES | 62.000 emails/tháng | $0 |
| AWS Amplify | 1000 build minutes/tháng, 5GB storage | $0 |

**Tổng chi phí ước tính: ~$0/tháng** trong giai đoạn đầu tiên (giai đoạn chứng minh năng lực MVP)

---

### 6. Đánh giá rủi ro

#### Rủi ro 1: Amazon SES Sandbox
- **Vấn đề**: Tín nhiệm gửi email của Amazon SES ở chế độ Sandbox bị giới hạn
- **Giải pháp**: Gửi yêu cầu lên bộ phận AWS Support để xin thoát khỏi chế độ Sandbox, xác thực Domain trước khi chính thức Launch hệ thống ra công chúng

#### Rủi ro 2: Lambda Cold Start
- **Vấn đề**: Vấn đề "Cold Start" của AWS Lambda khi không có người dùng thời gian dài, làm chậm tốc độ xuất PDF
- **Giải pháp**: Tối ưu hóa code Node.js, sử dụng các thư viện tạo PDF dung lượng nhẹ và set Provisioned Concurrency nếu cần thiết ở giai đoạn scale

---

### 7. Kết quả kỳ vọng

- Nền tảng Serverless hoàn chỉnh với chi phí vận hành gần bằng $0
- Hệ thống tự động scale khi có lượng truy cập lớn
- Báo cáo PDF được tạo và gửi email tự động trong vài giây
- Giao diện thân thiện với Gen Z
