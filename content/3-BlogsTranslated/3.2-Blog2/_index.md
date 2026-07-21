---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Blog 2

## Cara Pioneers Domain-Specific AI for Enterprise Insurance Brokerages with AWS

Insurance is an $8 trillion global industry, but it is currently bearing heavy burdens from outdated manual processes and worsening talent shortages. To solve this problem, Cara has pioneered developing an AI-native solution on the AWS platform to automate complex back-office processes specifically for enterprise insurance brokerages.

This article will analyze in depth the industry-specific challenges, the technical architecture of the solution, as well as the real results Cara achieved when collaborating with AWS.

---

## Major Challenges in Enterprise Insurance Industry

Insurance agents and brokers often spend hours on repetitive tasks: completing registration forms, analyzing and comparing coverage scopes between carriers, manually entering data across legacy systems, and continuously forwarding information between customers and insurers. In the context of prolonged talent shortages, brokerage companies need to increase revenue without proportionally expanding their teams.

However, general-purpose AI tools are difficult to meet this industry's requirements due to:

1. Strict regulatory controls: Requiring absolute accuracy, data auditability, and strict legal compliance.
2. Extremely sensitive data: Systems must handle personally identifiable information (PII), corporate financial records, and complex guarantee terms.
3. Domain-specific contextual requirements: AI must deeply understand insurance data models, specific requirements of each carrier, and risk appetites of each product portfolio.

---

## Cara's Journey and Vision

Cara's founding team are all experts with deep practical experience from successfully building and selling a large digital insurance brokerage company. From developing an internal AI Copilot tool to optimize their own processes, they recognized the outstanding efficiency and decided to build Cara as a domain-specific AI platform serving the entire insurance industry.

---

## Comprehensive Solution Architecture on AWS Platform

Cara is designed and deployed on AWS core services to optimize flexible scalability, reliability, and maximum security for businesses.

### 1. Compute & Orchestration

Cara operates entirely on Amazon Elastic Kubernetes Service (Amazon EKS) to manage and orchestrate microservices across multiple different Availability Zones (AZs).

- Multi-tenant isolation: To serve thousands of users, each brokerage organization's workloads run in independent Kubernetes Namespaces for absolute resource isolation.
- Auto-scaling: The system integrates auto-scaling tools to adjust processing capacity according to actual system load.

### 2. AI & Inference

Cara leverages the power of Amazon Bedrock to access leading large language models (LLMs) through a Fully Managed API, eliminating the GPU infrastructure management burden. The system applies Bedrock for core features including:

- Coverage analysis and quoting: Automatically comparing quotes from multiple different insurers, summarizing benefit differences, and highlighting exclusions or coverage gaps.
- Form automation: Extracting data from source documents to automatically cross-fill ACORD forms and supplementary forms.
- Renewal proposal generation: Automatically generating branded brokerage proposal packages and renewal spreadsheets.

### 3. Data Isolation and Enterprise Security

Cara applies account-per-tenant deployment strategy on AWS. Each brokerage company's data at rest and in transit is fully encrypted and isolated in separate private spaces, strictly controlled through AWS Identity and Access Management (AWS IAM).

### 4. System Integrations

Cara connects seamlessly with popular industry Agency Management Systems (AMS) and CRM tools to automatically synchronize accounts, policies, and documents, completely eliminating duplicate data entry.

---

## Infrastructure Automation with Terraform

To ensure speed and consistency when onboarding new customers, Cara automates infrastructure provisioning through Infrastructure as Code (IaC) using Terraform.

Below is an example illustrating Terraform (HCL) source code structure for initializing resources:

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

Thanks to the flexible multi-AZ architecture combined with Kubernetes Horizontal Pod Autoscaler (HPA), the system automatically scales processing capacity during peak periods (such as year-end insurance renewal season), ensuring high availability without interruption.

---

## Measured Quantitative Results

The combination of Cara's domain-specific AI and AWS's powerful cloud infrastructure has delivered outstanding business results:

| Metric                      | Actual Results Achieved                                           |
| ---                         | ---                                                             |
| Time savings                | Saving ~10 hours/week/user thanks to automation of repetitive tasks and contextual knowledge retrieval. |
| Onboarding speed            | Large enterprises configured within hours; customized processes operational within days. |
| Processing capacity         | Simultaneously serving thousands of users and processing complex files for each brokerage company. |
| Adoption rate              | Widely trusted by hundreds of leading enterprise insurance agency and brokerage companies in the US. |

## Conclusion

Cara is a typical example of applying domain-specific AI on the AWS platform. By combining Amazon EKS for orchestration and Amazon Bedrock for inference, Cara has built a platform that is both powerful, secure, and easily scalable – meeting the stringent requirements of the insurance industry.

The solution not only helps brokerage companies save time and operational costs but also enables employees to focus on core value: building customer relationships.
