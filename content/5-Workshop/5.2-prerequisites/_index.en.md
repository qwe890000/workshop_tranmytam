---
title: "Prerequisites"
weight: 2
chapter: true
pre: " <b> 5.2. </b> "
---

# Prerequisites

## Requirements Before Starting

To complete this workshop successfully, please ensure you have prepared all necessary tools and accounts.

## 1. AWS Account

### Create AWS Account (if you don't have one)

1. Go to [aws.amazon.com](https://aws.amazon.com)
2. Click **Create an AWS Account**
3. Fill in email, password, and account name
4. Complete email verification and credit card information

### Cost Notes

> **⚠️ Important:** This workshop uses AWS services that may incur charges. However, AWS provides **Free Tier** with:
> - **Lambda:** 1 million requests/month free
> - **S3:** 5GB storage free
> - **CloudFront:** 1TB transfer free
> - **API Gateway:** 1 million API calls/month free

## 2. AWS CLI v2

AWS CLI is the command line tool to interact with AWS services.

### Install on Windows

1. Download AWS CLI MSI installer: [AWS CLI MSI Download](https://awscli.amazonaws.com/AWSCLIV2.msi)
2. Run `AWSCLIV2.msi` and follow the instructions
3. Open Command Prompt and verify:

```bash
aws --version
# Output: aws-cli/2.x.x python...
```

### Install on macOS/Linux

```bash
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

### Configure AWS Credentials

1. Log in to AWS Console
2. Go to **IAM** → **Users** → **Create user**
3. Set user name: `lunagenz-admin`
4. Attach policies: `AdministratorAccess`
5. After creation, go to **Security credentials** tab
6. Click **Create access key**
7. Configure locally:

```bash
aws configure
# AWS Access Key ID [None]: your-access-key
# AWS Secret Access Key [None]: your-secret-key
# Default region name [None]: ap-southeast-1
# Default output format [None]: json
```

## 3. Git

Git is used to clone source code and manage versions.

### Installation

```bash
# Windows (using Chocolatey)
choco install git

# macOS
brew install git

# Verify installation
git --version
```

## 4. VS Code

VS Code is the recommended code editor.

### Download and Install

1. Download from [code.visualstudio.com](https://code.visualstudio.com)
2. Install extensions:
   - **AWS Toolkit** - Integration with AWS services
   - **Prettier** - Automatic code formatting
   - **Live Server** - Preview HTML/CSS

## 5. Source Code

Clone or download the LunaGenZ project source code:

```bash
# Clone from repository
git clone https://github.com/your-repo/lunagenz.git

# Or download directly from instructor
```

### Project Directory Structure

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

## Environment Check

Run the check script to ensure everything works:

```bash
# Check AWS credentials
aws sts get-caller-identity

# Check Git
git --version

# Check Node.js (if needed for local testing)
node --version
```

If everything works, you will see information about your current AWS account.

---

## Next Steps

Proceed to **S3 Hosting** section to start deploying the Frontend.
