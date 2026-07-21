---
title: "Điều kiện tiên quyết"
weight: 2
chapter: true
pre: " <b> 5.2. </b> "
---

# Điều kiện tiên quyết

## Các yêu cầu trước khi bắt đầu

Để hoàn thành tốt workshop này, vui lòng đảm bảo bạn đã chuẩn bị sẵn các công cụ và tài khoản cần thiết.

## 1. Tài khoản AWS

### Tạo tài khoản AWS (nếu chưa có)

1. Truy cập [aws.amazon.com](https://aws.amazon.com)
2. Nhấn **Create an AWS Account**
3. Điền thông tin email, mật khẩu, và tên tài khoản
4. Hoàn tất xác minh email và thông tin thẻ tín dụng

### Lưu ý về chi phí

> **⚠️ Quan trọng:** Workshop này sử dụng các dịch vụ AWS có thể phát sinh phí. Tuy nhiên, AWS cung cấp **Free Tier** với:
> - **Lambda:** 1 triệu request/tháng miễn phí
> - **S3:** 5GB storage miễn phí
> - **CloudFront:** 1TB transfer miễn phí
> - **API Gateway:** 1 triệu call API/tháng miễn phí

## 2. AWS CLI v2

AWS CLI là công cụ dòng lệnh để tương tác với AWS services.

### Cài đặt trên Windows

1. Tải AWS CLI MSI installer: [AWS CLI MSI Download](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Chạy file `AWSCLIV2.msi` và làm theo hướng dẫn
3. Mở Command Prompt và kiểm tra:

```bash
aws --version
# Output: aws-cli/2.x.x python...
```

### Cài đặt trên macOS/Linux

```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### Cấu hình AWS Credentials

1. Đăng nhập AWS Console
2. Truy cập **IAM** → **Users** → **Create user**
3. Đặt tên user: `lunagenz-admin`
4. Attach policies: `AdministratorAccess`
5. Sau khi tạo, vào tab **Security credentials**
6. Nhấn **Create access key**
7. Cấu hình local:

```bash
aws configure
# AWS Access Key ID [None]: your-access-key
# AWS Secret Access Key [None]: your-secret-key
# Default region name [None]: ap-southeast-1
# Default output format [None]: json
```

## 3. Git

Git dùng để clone source code và quản lý version.

### Cài đặt

```bash
# Windows (dùng Chocolatey)
choco install git

# macOS
brew install git

# Kiểm tra cài đặt
git --version
```

## 4. VS Code

VS Code là trình soạn thảo code được khuyến nghị.

### Tải và cài đặt

1. Tải từ [code.visualstudio.com](https://code.visualstudio.com)
2. Cài đặt extensions:
   - **AWS Toolkit** - Tích hợp với AWS services
   - **Prettier** - Format code tự động
   - **Live Server** - Preview HTML/CSS

## 5. Source Code

Clone hoặc tải source code của dự án LunaGenZ:

```bash
# Clone từ repository
git clone https://github.com/your-repo/lunagenz.git

# Hoặc tải trực tiếp từ instructor
```

### Cấu trúc thư mục dự án

```
lunagenz/
├── frontend/           # HTML, CSS, JavaScript files
│   ├── index.html
│   ├── styles.css
│   ├── app.js
│   └── assets/
├── backend/            # Lambda function code
│   ├── calculation/
│   │   ├── index.js
│   │   └── package.json
│   └── pdf-generator/
│       ├── index.js
│       └── package.json
└── scripts/           # Deployment scripts
```

## Kiểm tra môi trường

Chạy script kiểm tra để đảm bảo mọi thứ hoạt động:

```bash
# Kiểm tra AWS credentials
aws sts get-caller-identity

# Kiểm tra Git
git --version

# Kiểm tra Node.js (nếu cần cho local testing)
node --version
```

Nếu mọi thứ hoạt động tốt, bạn sẽ thấy thông tin về AWS account hiện tại.

---

## Các bước tiếp theo

Chuyển sang phần **S3 Hosting** để bắt đầu triển khai Frontend.
