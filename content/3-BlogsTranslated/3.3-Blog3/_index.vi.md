---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Blog 3

## Xây dựng hệ thống Thần Số Học (LunaGENZ) trên kiến trúc Serverless AWS kết hợp GenAI

Chào mọi người, sau một thời gian tự học và cày thì team mình cũng bắt đầu làm đồ án cuối khóa: LunaGENZ – hệ thống dùng AI để luận giải Thần số học cá nhân hóa.

Mình viết bài này không phải để khoe, mà muốn ghi lại quá trình ra quyết định kiến trúc, kèm những lỗi đã gặp – hy vọng có ích cho bạn nào đang làm đồ án tương tự, và rất mong nhận góp ý từ các anh chị có kinh nghiệm để team rút kinh nghiệm thêm.

## 1. Vì sao chọn kiến trúc Serverless

Mục tiêu ban đầu là tránh phải tự quản lý server, đồng thời giữ chi phí ở mức phù hợp với một dự án sinh viên không có doanh thu ổn định. Team chọn:

- Frontend: Next.js, deploy qua AWS Amplify
- Backend: API Gateway + AWS Lambda
- Database: Amazon DynamoDB (chế độ On-demand)

Mô hình trả tiền theo lượng dùng thực tế giúp chi phí hạ tầng trong giai đoạn test rất thấp, phù hợp với một sản phẩm chưa có người dùng thật. Đây là điểm phù hợp với bài toán cụ thể của team ở giai đoạn này, không phải là kết luận chung cho mọi dự án — khi traffic tăng, mô hình serverless cũng có những đánh đổi riêng về chi phí mà team vẫn đang tìm hiểu thêm.

## 2. Vấn đề Timeout và cách xử lý bằng Amazon SQS

Ban đầu hệ thống gọi AI sinh nội dung và xuất PDF ngay trong cùng một request. Vì các bước này tốn thời gian, hệ thống liên tục dính lỗi timeout do API Gateway giới hạn 29 giây.

Cách team xử lý: tách luồng xử lý bằng Amazon SQS. Request từ người dùng chỉ đẩy message vào hàng đợi và trả về phản hồi "đang xử lý". Một Lambda khác chạy nền sẽ lấy message ra, gọi AI, tạo PDF, rồi gửi qua Amazon SES về email người dùng. Cách này giúp hệ thống không còn timeout và giảm rủi ro mất dữ liệu khi xử lý lỗi.

## 3. Lựa chọn model AI

Team dùng Claude 3 Haiku qua Amazon Bedrock thay vì các model lớn hơn. Lý do là bài toán luận giải text tiếng Việt của tụi mình không đòi hỏi reasoning quá phức tạp, nên một model nhỏ, phản hồi nhanh, chi phí thấp hơn là lựa chọn hợp lý hơn so với việc dùng model mạnh nhất có thể.

## 4. Bảo mật và CI/CD

- Luồng thanh toán được tách thành một Webhook Handler riêng, độc lập với phần luận giải AI.
- Các key nhạy cảm lưu trong AWS Secrets Manager, không hardcode trong code.
- Toàn bộ frontend và backend được tự động deploy qua GitLab CI/CD. Theo dõi lỗi và performance bằng CloudWatch.

## Vài điều mình và team rút ra được

Đây là lần đầu team áp dụng kiến trúc serverless ở mức tương đối đầy đủ, nên chắc chắn còn nhiều điểm chưa tối ưu — ví dụ việc xử lý lỗi khi Lambda xử lý nền bị fail, hay cách kiểm soát chi phí khi scale thật vẫn là những bài toán team chưa thử nghiệm ở quy mô lớn.

Do cũng là lần đầu làm một project mà liên quan đến ngoài nên cũng không bỏ qua nhiều sai sót, mong là các anh chị sẽ cùng góp gạch giúp tụi em để tụi em làm tốt hơn ạ. Em cảm ơn mọi người đã bỏ thời gian đọc bài viết này ạ.
