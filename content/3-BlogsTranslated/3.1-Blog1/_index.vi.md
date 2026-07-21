---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Blog 1

## Tự động hóa triển khai Oracle Database@AWS bằng Terraform

Việc triển khai các hệ thống cơ sở dữ liệu Oracle trên nền tảng đám mây luôn là bài toán đòi hỏi sự cân bằng giữa hiệu năng, khả năng mở rộng và tính nhất quán trong quản lý hạ tầng. Với Oracle Database@AWS (ODB@AWS), Oracle đã mang nền tảng Exadata – hệ thống phần cứng được tối ưu dành riêng cho Oracle Database – vào trực tiếp trung tâm dữ liệu của AWS, cho phép doanh nghiệp khai thác hiệu năng cao của Oracle đồng thời tận dụng hệ sinh thái dịch vụ phong phú của AWS.

Tuy nhiên, để xây dựng hoàn chỉnh một môi trường Oracle Database@AWS, người quản trị phải thực hiện nhiều bước cấu hình như thiết lập ODB Network, triển khai Exadata Infrastructure, tạo VM Cluster và kết nối với Amazon VPC. Nếu thực hiện hoàn toàn trên AWS Console, quy trình này khá phức tạp, mất nhiều thời gian và dễ xảy ra sai sót khi số lượng môi trường triển khai tăng lên.

Trong bài viết này, chúng ta sẽ tìm hiểu cách sử dụng Terraform để tự động hóa toàn bộ quá trình triển khai Oracle Database@AWS. Thông qua mô hình Infrastructure as Code (IaC), doanh nghiệp có thể chuẩn hóa hạ tầng, giảm thiểu lỗi cấu hình và triển khai hệ thống một cách nhanh chóng, nhất quán.

---

## Hướng dẫn kiến trúc

Oracle Database@AWS được thiết kế để kết hợp hiệu năng của Oracle Exadata với khả năng mở rộng và tính linh hoạt của AWS. Thay vì vận hành Oracle Database trên hạ tầng riêng biệt, doanh nghiệp có thể triển khai trực tiếp trên cơ sở hạ tầng AWS nhưng vẫn sử dụng các tính năng tối ưu của Exadata.

Trong mô hình này, Terraform đóng vai trò là công cụ điều phối toàn bộ quá trình tạo tài nguyên. Các thành phần hạ tầng được mô tả bằng mã nguồn HCL và được Terraform tự động triển khai theo đúng thứ tự phụ thuộc, giúp đảm bảo tính nhất quán giữa các môi trường.

Giải pháp bao gồm bốn thành phần chính:

* ODB Network: Mạng chuyên dụng kết nối giữa AWS và Oracle Cloud Infrastructure (OCI), cung cấp đường truyền riêng có độ trễ thấp.
* Oracle Exadata Infrastructure: Hạ tầng phần cứng Exadata được triển khai trực tiếp trong Availability Zone của AWS để cung cấp khả năng xử lý dữ liệu hiệu năng cao.
* Exadata VM Cluster: Cụm máy ảo chạy Oracle Grid Infrastructure và Oracle Database.
* ODB Peering Connection: Kết nối mạng riêng giữa Amazon VPC và ODB Network, cho phép các ứng dụng trên AWS truy cập Oracle Database một cách an toàn.

> _Hình 1. Kiến trúc tổng thể; những ô màu thể hiện những dịch vụ riêng biệt._

Sau khi các thành phần trên được tạo thành công, doanh nghiệp có thể triển khai Oracle Database với đầy đủ khả năng mở rộng, tính sẵn sàng cao và khả năng tích hợp với các dịch vụ AWS.

---

## Vì sao nên sử dụng Terraform?

Terraform là một trong những công cụ Infrastructure as Code phổ biến nhất hiện nay. Thay vì thao tác trực tiếp trên giao diện quản trị AWS, toàn bộ hạ tầng được mô tả dưới dạng mã nguồn HCL (HashiCorp Configuration Language). Terraform sẽ đọc các tệp cấu hình này và tự động tạo tài nguyên theo đúng trình tự phụ thuộc.

Việc áp dụng Terraform mang lại nhiều lợi ích:

* Tự động hóa hoàn toàn quá trình triển khai hạ tầng.
* Loại bỏ các thao tác cấu hình thủ công trên AWS Console.
* Đảm bảo cấu hình đồng nhất giữa các môi trường Development, Testing và Production.
* Dễ dàng quản lý thay đổi thông qua Git.
* Có thể tái sử dụng cấu hình cho nhiều dự án khác nhau.
* Hỗ trợ mở rộng và cập nhật hạ tầng mà không ảnh hưởng đến các tài nguyên hiện có.

Đối với các tổ chức triển khai nhiều hệ thống Oracle Database hoặc áp dụng DevOps, Terraform giúp giảm đáng kể thời gian triển khai và tăng khả năng kiểm soát hạ tầng.

---

## Điều kiện trước khi triển khai

Trước khi chạy Terraform, cần đảm bảo đã hoàn tất các bước chuẩn bị sau:

### 1 Đăng ký Oracle Database@AWS

Doanh nghiệp cần đăng ký dịch vụ Oracle Database@AWS thông qua AWS Marketplace và hoàn tất quy trình Onboarding với Oracle.

### 2 Liên kết tài khoản

AWS Account phải được liên kết với Oracle Cloud Infrastructure (OCI Tenancy) để Terraform có quyền quản lý tài nguyên trên cả hai nền tảng.

Cấu hình Terraform Provider

Terraform cần cấu hình đồng thời hai Provider:

* AWS Provider
* OCI Provider

Hai Provider này cho phép Terraform giao tiếp với API của AWS và Oracle Cloud.

### 3 Thiết lập quyền IAM

Tài khoản sử dụng Terraform cần có đầy đủ quyền tạo, cập nhật và xóa tài nguyên trên AWS cũng như Oracle Cloud.

Sau khi hoàn tất các điều kiện trên, người quản trị có thể bắt đầu triển khai hạ tầng Oracle Database@AWS bằng Terraform.

---

## Quy trình triển khai bằng Terraform

Terraform sẽ tạo các thành phần của Oracle Database@AWS theo đúng trình tự phụ thuộc. Mỗi thành phần được định nghĩa dưới dạng một Resource trong Terraform.

### Bước 1. Khởi tạo ODB Network

Bước đầu tiên là tạo Oracle Database Network, đóng vai trò thiết lập kết nối mạng riêng giữa AWS và Oracle Cloud Infrastructure.

Ví dụ cấu hình Terraform:

```hcl
resource "aws_odb_network" "example" {
  display_name         = "odb-my-net"
  availability_zone_id = "use1-az6"
  client_subnet_cidr   = "10.2.0.0/24"
  backup_subnet_cidr   = "10.2.1.0/24"
  s3_access            = "DISABLED"
  zero_etgl_access     = "DISABLED"
  tags = {
    "env" = "dev"
  }
}
```

#### Trong cấu hình trên:

* display_name xác định tên của ODB Network.
* availability_zone_id chỉ định Availability Zone sẽ triển khai.
* client_subnet_cidr và backup_subnet_cidr định nghĩa hai dải mạng dành cho lưu lượng ứng dụng và sao lưu.
* s3_access cho phép Oracle Database truy cập Amazon S3.
* zero_etl_access kích hoạt khả năng tích hợp Zero-ETL.

Sau khi hoàn thành bước này, toàn bộ các tài nguyên Oracle Database sẽ được triển khai trên ODB Network vừa tạo.

### Bước 2. Triển khai Oracle Exadata Infrastructure

Tiếp theo, Terraform khởi tạo hạ tầng phần cứng Exadata.

```hcl
resource "aws_odb_cloud_exadata_infrastructure" "example" {
  display_name         = "my-exa-infra"
  availability_zone    = "use1-az6"
  shape                = "exadata.oci.x11m"
  database_server_type = "X11M"
  storage_server_type  = "X11M-HC"
  maintenance_window {
    custom_action_timeout_in_mins = 16
    days_of_week = [{ name = "MONDAY" }, { name = "TUESDAY" }]
    hours_of_day = [11, 16]
    is_custom_action_timeout_enabled = true
    lead_time_in_weeks = 3
    months = [{ name = "FEBRUARY" }, { name = "MAY" }, { name = "AUGUST" }, { name = "NOVEMBER" }]
    patching_mode = "ROLLING"
    preference = "CUSTOM_PREFERENCE"
    weeks_of_month = [2, 4]
  }
  tags = {
    "env" = "dev"
  }
}
```

Oracle Exadata Infrastructure cung cấp nền tảng phần cứng chuyên dụng để vận hành Oracle Database với hiệu năng cao.

Tại bước này, người quản trị có thể lựa chọn cấu hình phù hợp với nhu cầu sử dụng như:

* Compute Node
* Storage Server
* Dung lượng lưu trữ
* Availability Zone

Terraform sẽ tự động liên kết Exadata Infrastructure với ODB Network đã tạo ở bước trước.

### Bước 3. Khởi tạo Exadata VM Cluster

Sau khi Exadata Infrastructure sẵn sàng, Terraform tiếp tục triển khai VM Cluster.

```hcl
resource "aws_odb_cloud_vm_cluster" "example" {
  display_name                    = "my-vm-cluster"
  cloud_exadata_infrastructure_id = "<exadata-infra-id>"
  odb_network_id                  = "<odb-network-id>"
  cpu_core_count                  = 6
  gi_version                      = "23.0.0.0"
  hostname_prefix                 = "apollo12"
  ssh_public_keys                 = ["your-ssh-public-key"]
  license_model                   = "LICENSE_INCLUDED"
  data_storage_size_in_tbs        = 20.0
  db_servers                      = ["db-server-1", "db-server-2"]
  db_node_storage_size_in_gbs     = 120.0
  memory_size_in_gbs              = 60
  tags = {
    "env" = "dev"
  }
}
```

VM Cluster là môi trường trực tiếp chạy Oracle Database.

Tại đây, người quản trị có thể cấu hình:

* Số lượng CPU Core.
* Bộ nhớ (Memory).
* Oracle Grid Infrastructure.
* Oracle Home.
* Phiên bản Oracle Database.

Nhờ Terraform, toàn bộ các thông số này được lưu trữ dưới dạng mã nguồn, giúp dễ dàng mở rộng hoặc tái sử dụng cho nhiều môi trường khác nhau.

### Bước 4. Thiết lập ODB Peering Connection

Sau khi Oracle Database Infrastructure được tạo, bước cuối cùng là thiết lập kết nối mạng giữa Amazon VPC và ODB Network.

```hcl
resource "aws_odb_network_peering_connection" "example" {
  display_name   = "example-peering"
  odb_network_id = "<odb-network-id>"
  peer_network_id = "<vpc-id>"
  tags = {
    "env" = "dev"
  }
}
```

Peering Connection cho phép các ứng dụng triển khai trên Amazon EC2, Amazon ECS hoặc Amazon EKS truy cập Oracle Database thông qua kết nối mạng riêng, giảm độ trễ và tăng cường bảo mật.

Sau khi Terraform hoàn tất bước này, toàn bộ hạ tầng Oracle Database@AWS đã sẵn sàng để triển khai và vận hành cơ sở dữ liệu.

---

## Kiến trúc sau khi triển khai

Sau khi Terraform hoàn tất quá trình provisioning, toàn bộ hạ tầng Oracle Database@AWS sẽ được tạo theo đúng trình tự phụ thuộc. Terraform tự động quản lý các mối quan hệ giữa tài nguyên, giúp người quản trị không cần thao tác thủ công trên AWS Console.

Kiến trúc sau khi triển khai bao gồm các thành phần sau:

| Thành phần                    | Vai trò                                                                 |
| ----------------------------- | ----------------------------------------------------------------------- |
| ODB Network                   | Thiết lập mạng riêng kết nối AWS với Oracle Cloud Infrastructure (OCI). |
| Oracle Exadata Infrastructure | Cung cấp hạ tầng phần cứng Exadata phục vụ Oracle Database.             |
| Oracle Exadata VM Cluster     | Môi trường chạy Oracle Grid Infrastructure và Oracle Database.          |
| ODB Peering Connection        | Kết nối Amazon VPC với ODB Network để ứng dụng truy cập Database.       |
| AWS Provider                  | Quản lý và triển khai tài nguyên trên AWS.                              |
| OCI Provider                  | Quản lý tài nguyên Oracle Cloud Infrastructure.                         |

Toàn bộ tài nguyên được Terraform quản lý trong **Terraform State**, giúp dễ dàng theo dõi trạng thái hạ tầng, cập nhật hoặc mở rộng hệ thống trong tương lai.

---

## Triển khai dự án bằng Terraform

AWS đã cung cấp một dự án mẫu trên GitHub giúp người dùng nhanh chóng triển khai Oracle Database@AWS bằng Terraform.

Đầu tiên, tải mã nguồn:

```bash
git clone https://github.com/aws-samples/sample-odb-launch-using-terraform.git

cd sample-odb-launch-using-terraform
```

Sau khi tải mã nguồn, khởi tạo Terraform:

```bash
terraform init
```

Lệnh trên sẽ tải các Provider cần thiết như AWS Provider và OCI Provider.

Tiếp theo, kiểm tra kế hoạch triển khai:

```bash
terraform plan
```

Terraform sẽ phân tích toàn bộ mã nguồn và hiển thị danh sách các tài nguyên sẽ được tạo, cập nhật hoặc xóa.

Nếu mọi cấu hình đều chính xác, tiến hành triển khai:

```bash
terraform apply
```

Terraform sẽ lần lượt thực hiện:

1. Tạo ODB Network.
2. Khởi tạo Oracle Exadata Infrastructure.
3. Tạo Oracle Exadata VM Cluster.
4. Thiết lập ODB Peering Connection.
5. Cập nhật Terraform State.

Sau khi hoàn tất, môi trường Oracle Database@AWS đã sẵn sàng để triển khai Database và kết nối với các ứng dụng trên AWS.

---

## Best Practices khi sử dụng Terraform

Đối với các hệ thống Oracle Database trong môi trường doanh nghiệp, AWS khuyến nghị áp dụng một số thực tiễn tốt nhằm tăng tính ổn định và khả năng quản lý.

### 1. Luôn chạy `terraform plan` trước `terraform apply`

Việc kiểm tra kế hoạch triển khai giúp phát hiện sớm các thay đổi không mong muốn trước khi tác động đến môi trường Production.

### 2. Quản lý Terraform State từ xa

Không nên lưu file `terraform.tfstate` trên máy cá nhân.

Thay vào đó nên sử dụng:

* Amazon S3 để lưu State.
* Amazon DynamoDB để khóa State (State Locking).

Điều này giúp nhiều thành viên trong nhóm có thể làm việc đồng thời mà không xảy ra xung đột.

### 3. Quản lý mã nguồn bằng Git

Toàn bộ mã Terraform nên được lưu trữ trên GitHub hoặc GitLab nhằm:

* Theo dõi lịch sử thay đổi.
* Thực hiện Code Review.
* Khôi phục cấu hình khi cần thiết.
* Tích hợp CI/CD.

### 4. Tách riêng các môi trường

Nên sử dụng biến (`variables`) hoặc Terraform Workspace để tách biệt các môi trường:

* Development
* Testing
* Staging
* Production

Việc này giúp hạn chế ảnh hưởng giữa các môi trường và đơn giản hóa quá trình triển khai.

### 5. Hạn chế thay đổi trực tiếp trên AWS Console

Sau khi áp dụng Infrastructure as Code, mọi thay đổi nên được thực hiện thông qua Terraform thay vì chỉnh sửa trực tiếp trên AWS Console. Điều này giúp tránh tình trạng **Configuration Drift**, đảm bảo hạ tầng thực tế luôn đồng nhất với mã nguồn.

---

## Đối tượng phù hợp

Giải pháp Oracle Database@AWS kết hợp Terraform phù hợp với nhiều nhóm kỹ thuật khác nhau.

* **Database Administrator (DBA):** Tự động hóa quá trình triển khai và quản trị Oracle Database.
* **Cloud Engineer:** Chuẩn hóa hạ tầng Oracle trên AWS theo mô hình Infrastructure as Code.
* **DevOps Engineer:** Tích hợp triển khai cơ sở dữ liệu vào quy trình CI/CD.
* **Enterprise Architect:** Thiết kế hạ tầng Oracle Database có khả năng mở rộng và dễ bảo trì.
* **Doanh nghiệp Enterprise:** Các tổ chức đang chuyển đổi hệ thống Oracle Database từ On-premises lên AWS nhưng vẫn yêu cầu hiệu năng cao của Oracle Exadata.

---

## Kết luận

Oracle Database@AWS mang đến một hướng tiếp cận mới cho việc triển khai Oracle Database trên nền tảng đám mây bằng cách kết hợp hiệu năng của Oracle Exadata với hệ sinh thái dịch vụ AWS. Tuy nhiên, đi kèm với đó là quy trình cấu hình nhiều thành phần hạ tầng và yêu cầu tính nhất quán trong quá trình triển khai.

Terraform giúp giải quyết bài toán này thông qua mô hình **Infrastructure as Code**, cho phép toàn bộ hạ tầng được định nghĩa bằng mã nguồn thay vì thao tác thủ công trên giao diện quản trị. Nhờ khả năng tự động hóa, quản lý phiên bản và tái sử dụng cấu hình, Terraform không chỉ rút ngắn thời gian triển khai mà còn giảm thiểu rủi ro do lỗi cấu hình và nâng cao hiệu quả vận hành hệ thống.

Đối với các doanh nghiệp đang xây dựng hoặc hiện đại hóa hạ tầng Oracle Database trên AWS, việc kết hợp Oracle Database@AWS với Terraform là một giải pháp phù hợp để chuẩn hóa quy trình triển khai, tăng khả năng mở rộng và đơn giản hóa công tác quản trị trong dài hạn.

Nguồn: AWS Database Blog - Provision Oracle Database@AWS resources using Terraform.
