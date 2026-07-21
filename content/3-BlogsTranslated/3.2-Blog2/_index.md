---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

---

## Cara Pioneers Domain-Specific AI for Enterprise Insurance Brokerages with AWS

Insurance is a global industry worth $8 trillion, but it is currently bearing heavy pressure from outdated manual processes and increasingly severe talent shortages. To solve this problem, Cara pioneered developing an AI-native solution on the AWS platform to automate complex back-office processes specifically for enterprise insurance brokerages.

This article will analyze in depth the unique challenges of the industry, the technical architecture of the solution, as well as the practical results that Cara achieved when collaborating with AWS.

---

## Major Challenges of the Enterprise Insurance Industry

Insurance agents and brokers often have to spend hours on repetitive tasks: completing registration forms, analyzing and comparing coverage across insurers, manually entering data through multiple legacy systems, and continuously forwarding information between customers and insurers. In the context of prolonged staffing shortages, brokerage companies need to increase revenue without proportionally expanding their teams.

However, general AI tools are very difficult to meet the industry's requirements because:

1. **Strict regulatory controls:** Require absolute accuracy, data auditability, and strict legal compliance.
2. **Extremely sensitive data:** The system must process personally identifiable information (PII), enterprise financial records, and complex guarantee terms.
3. **Domain-specific contextual requirements:** AI must deeply understand insurance data models, specific requirements of each insurer, and risk appetite for each product portfolio.

---

## Cara's Journey and Vision

Cara's founding team are all experts with deep practical experience from successfully building and selling a large digital insurance brokerage company. From developing an internal AI Copilot tool to optimize their own processes, they recognized the superior efficiency and decided to build Cara as a domain-specific AI platform serving the entire insurance industry.

---

## Comprehensive Solution Architecture on AWS Platform

Cara is designed and deployed on AWS core services to optimize scalability, reliability, and maximum security for enterprises.

### 1. Compute & Orchestration

Cara operates entirely on Amazon Elastic Kubernetes Service (Amazon EKS) to manage and orchestrate microservices across multiple different Availability Zones (AZ).

- **Multi-tenant isolation:** To serve thousands of users, each brokerage organization's workload is run in independent Kubernetes Namespaces to ensure absolute resource isolation.
- **Auto-scaling:** The system integrates auto-scaling tools to adjust processing capacity according to actual system load.

### 2. AI & Inference

Cara leverages the power of Amazon Bedrock to access leading large language models (LLM) through a fully managed API, eliminating GPU infrastructure management burden. The system applies Bedrock for core features including:

- **Coverage analysis and quotation:** Automatically compare quotations from multiple different insurers, summarize benefit differences, and highlight exclusions or coverage gaps.
- **Form automation:** Extract data from source documents to automatically cross-fill ACORD forms and supplementary forms.
- **Renewal proposal generation:** Automatically output proposal packages branded for the broker and renewal spreadsheets.

### 3. Data Isolation and Enterprise Security

Cara applies per-account deployment strategy on AWS. Static (at rest) and in-transit data of each brokerage company is fully encrypted and isolated in separate secure spaces, strictly controlled through AWS Identity and Access Management (AWS IAM).

### 4. System Integrations

Cara connects seamlessly with popular industry Agent Management Systems (AMS) and CRM tools to automatically synchronize accounts, policies, and documents, completely eliminating duplicate data entry.

---

## Automating Infrastructure Deployment with Terraform

To ensure speed and consistency when onboarding new customers, Cara automates infrastructure provisioning through Infrastructure as Code (IaC) using Terraform.

Below is an example illustrating the Terraform (HCL) source code structure for initializing resources:

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

Thanks to the flexible multi-AZ architecture combined with Kubernetes Horizontal Pod Autoscaler (HPA), the system automatically scales processing capacity during peak periods (such as year-end insurance renewal season), ensuring high availability and no interruptions.

---

## Actual Quantitative Results

The combination of Cara's domain-specific AI and AWS's powerful cloud infrastructure has delivered superior business results:

| Measurement Metric | Actual Results Achieved |
| --- | --- |
| Time Savings | Saves an average of ~10 hours/week/user thanks to automation of repetitive tasks and contextual knowledge retrieval. |
| Onboarding Speed | Large enterprises configured within hours; customized processes operational within days. |
| Processing Capacity | Handles thousands of simultaneous users and complex application processing for each brokerage company. |
| Adoption Level | Widely trusted by hundreds of top enterprise agent and insurance brokerage companies in the US. |

## Conclusion

Cara is a typical example of applying domain-specific AI on the AWS platform. By combining Amazon EKS for orchestration and Amazon Bedrock for inference, Cara has built a platform that is both powerful, secure, and easily scalable – meeting the exacting requirements of the insurance industry.

The solution not only helps brokerage companies save time and operating costs but also helps employees focus on core value: building customer relationships.
