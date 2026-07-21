---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Blog 2

## Cara tiên phong AI chuyên ngành cho môi giới bảo hiểm doanh nghiệp với AWS

Bảo hiểm là một ngành công nghiệp toàn cầu trị giá 8 nghìn tỷ USD, nhưng hiện đang phải gánh chịu áp lực nặng nề bởi các quy trình thủ công lỗi thời và tình trạng thiếu hụt nhân tài ngày càng trầm trọng. Để giải quyết bài toán này, Cara đã tiên phong phát triển một giải pháp AI-native trên nền tảng AWS nhằm tự động hóa các quy trình back-office phức tạp dành riêng cho các công ty môi giới bảo hiểm doanh nghiệp.

Bài viết này sẽ đi sâu phân tích những thách thức đặc thù của ngành, kiến trúc kỹ thuật của giải pháp, cũng như kết quả thực tế mà Cara đạt được khi hợp tác cùng AWS.

---

## Thách thức lớn của ngành bảo hiểm doanh nghiệp

Các đại lý và chuyên viên môi giới bảo hiểm thường xuyên phải dành hàng giờ cho các tác vụ lặp đi lặp lại: hoàn thiện hồ sơ đăng ký, phân tích và so sánh phạm vi bảo hiểm giữa các hãng, nhập liệu thủ công qua nhiều hệ thống cũ kỹ, và liên tục chuyển tiếp thông tin giữa khách hàng và nhà bảo hiểm. Trong bối cảnh thiếu hụt nhân sự kéo dài, các công ty môi giới cần tìm cách gia tăng doanh thu mà không phải mở rộng quy mô đội ngũ theo tỷ lệ thuận.

Tuy nhiên, các công cụ AI phổ thông (General AI) rất khó đáp ứng được yêu cầu của ngành này do:

1. Quy định kiểm soát khắt khe: Đòi hỏi độ chính xác tuyệt đối, khả năng kiểm toán dữ liệu và tuân thủ pháp lý nghiêm ngặt.
2. Dữ liệu cực kỳ nhạy cảm: Hệ thống phải xử lý thông tin nhận dạng cá nhân (PII), hồ sơ tài chính doanh nghiệp và các điều khoản bảo lãnh phức tạp.
3. Yêu cầu ngữ cảnh chuyên ngành: AI phải hiểu sâu sắc về mô hình dữ liệu bảo hiểm, các yêu cầu riêng biệt của từng nhà bảo hiểm cũng như khẩu vị rủi ro của từng danh mục sản phẩm.

---

## Hành trình và tầm nhìn của Cara

Đội ngũ sáng lập Cara đều là những chuyên gia có kinh nghiệm thực tế sâu sắc từ việc xây dựng và bán thành công một công ty môi giới bảo hiểm kỹ thuật số lớn. Từ việc phát triển một công cụ AI Copilot nội bộ nhằm tối ưu hóa quy trình cho chính mình, họ đã nhận thấy hiệu quả vượt trội và quyết định xây dựng Cara như một nền tảng AI chuyên biệt (domain-specific AI) phục vụ cho toàn bộ ngành bảo hiểm.

---

## Kiến trúc giải pháp toàn diện trên nền tảng AWS

Cara được thiết kế và triển khai trên các dịch vụ cốt lõi của AWS nhằm tối ưu hóa khả năng mở rộng linh hoạt, độ tin cậy và bảo mật tối đa cho doanh nghiệp.

### 1. Tính toán và điều phối (Compute & Orchestration)

Cara vận hành hoàn toàn trên Amazon Elastic Kubernetes Service (Amazon EKS) để quản lý và điều phối các microservices trên nhiều Availability Zone (AZ) khác nhau.

- Cách ly Multi-tenant: Để phục vụ hàng nghìn người dùng, khối lượng công việc của từng tổ chức môi giới được chạy trong các Kubernetes Namespace độc lập nhằm cách ly tài nguyên tuyệt đối.
- Tự động co giãn: Hệ thống tích hợp các công cụ tự động scale để điều chỉnh năng lực xử lý theo tải thực tế của hệ thống.

### 2. Trí tuệ nhân tạo và Cơ chế suy luận (AI & Inference)

Cara tận dụng sức mạnh của Amazon Bedrock để truy cập các mô hình ngôn ngữ lớn (LLM) hàng đầu thông qua một API được quản lý hoàn toàn (Fully Managed), loại bỏ gánh nặng quản lý hạ tầng GPU. Hệ thống ứng dụng Bedrock cho các tính năng cốt lõi bao gồm:

- Phân tích phạm vi bảo hiểm và báo giá: Tự động so sánh các bản báo giá từ nhiều nhà bảo hiểm khác nhau, tóm tắt sự khác biệt về quyền lợi và làm nổi bật các điều khoản loại trừ hoặc khoảng trống bảo hiểm.
- Tự động hóa biểu mẫu: Trích xuất dữ liệu từ tài liệu nguồn để tự động điền chéo vào các biểu mẫu ACORD và form bổ sung.
- Tạo đề xuất tái tục: Tự động xuất ra các bộ hồ sơ đề xuất (proposals) mang thương hiệu của môi giới và bảng tính tái tục (renewal spreadsheets).

### 3. Cách ly dữ liệu và Bảo mật doanh nghiệp

Cara áp dụng chiến lược triển khai theo từng tài khoản (Account-per-tenant) trên AWS. Dữ liệu tĩnh (at rest) và dữ liệu truyền tải (in transit) của mỗi công ty môi giới đều được mã hóa toàn bộ và cô lập trong các không gian bảo mật riêng biệt, kiểm soát chặt chẽ thông qua AWS Identity and Access Management (AWS IAM).

### 4. Khả năng tích hợp hệ thống (Integrations)

Cara kết nối mượt mà với các Hệ thống quản lý đại lý (AMS) và công cụ CRM phổ biến trong ngành để đồng bộ hóa tài khoản, chính sách và tài liệu tự động, loại bỏ hoàn toàn việc nhập liệu trùng lặp.

---

## Tự động hóa triển khai hạ tầng với Terraform

Để đảm bảo tốc độ và tính nhất quán khi onboard khách hàng mới, Cara tự động hóa việc cấp phát hạ tầng bằng Infrastructure as Code (IaC) thông qua Terraform.

Dưới đây là ví dụ minh họa cấu trúc mã nguồn Terraform (HCL) dùng để khởi tạo tài nguyên:

```hcl
resource "kubernetes_namespace" "tenant_workspace" {
  metadata {
    name = "cara-tenant-${var.tenant_id}"
    labels = {
      environment = "production"
      tenant_type = "enterprise"
    }
  }
}

resource "aws_iam_role" "tenant_bedrock_access" {
  name = "cara-iam-role-${var.tenant_id}"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
      }
    ]
  })
}
```

Nhờ kiến trúc multi-AZ linh hoạt kết hợp với Kubernetes Horizontal Pod Autoscaler (HPA), hệ thống tự động co giãn năng lực xử lý trong các giai đoạn cao điểm (như mùa tái tục bảo hiểm cuối năm), đảm bảo tính sẵn sàng cao (High Availability) và không bị gián đoạn.

---

## Kết quả định lượng đo lường thực tế

Sự kết hợp giữa AI chuyên ngành của Cara và hạ tầng đám mây mạnh mẽ của AWS đã mang lại những kết quả kinh doanh vượt trội:

| Chỉ số đo lường | Kết quả thực tế đạt được |
| --- | --- |
| Thời gian tiết kiệm | Tiết kiệm trung bình ~10 giờ/tuần/người dùng nhờ tự động hóa tác vụ lặp lại và truy xuất kiến thức theo ngữ cảnh. |
| Tốc độ Onboarding | Doanh nghiệp lớn được cấu hình xong hệ thống trong vài giờ; các quy trình tùy chỉnh đi vào hoạt động trong vài ngày. |
| Năng lực xử lý | Đáp ứng đồng thời hàng nghìn người dùng và quy trình xử lý hồ sơ phức tạp cho mỗi công ty môi giới. |
| Mức độ chấp nhận | Được tin dùng rộng rãi bởi hàng trăm công ty đại lý và môi giới bảo hiểm doanh nghiệp hàng đầu tại Mỹ. |

## Kết luận

Cara là minh chứng điển hình cho việc áp dụng AI chuyên ngành (domain-specific AI) trên nền tảng AWS. Bằng cách kết hợp Amazon EKS cho orchestration và Amazon Bedrock cho inference, Cara đã xây dựng được một nền tảng vừa mạnh mẽ, vừa an toàn và dễ mở rộng – đáp ứng đúng nhu cầu khắt khe của ngành bảo hiểm.

Giải pháp không chỉ giúp các công ty môi giới tiết kiệm thời gian và chi phí vận hành mà còn giúp nhân viên tập trung vào giá trị cốt lõi: xây dựng mối quan hệ với khách hàng.
