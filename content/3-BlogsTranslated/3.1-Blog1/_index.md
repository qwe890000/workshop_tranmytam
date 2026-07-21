---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

---

## Automating Oracle Database@AWS Deployment with Terraform

Deploying Oracle Database systems on cloud platforms is always a challenge that requires balancing performance, scalability, and consistency in infrastructure management. With Oracle Database@AWS (ODB@AWS), Oracle has brought the Exadata platform – hardware systems optimized specifically for Oracle Database – directly into AWS data centers, allowing businesses to leverage Oracle's high performance while taking advantage of AWS's rich service ecosystem.

However, to fully build an Oracle Database@AWS environment, administrators must perform multiple configuration steps such as setting up ODB Network, deploying Exadata Infrastructure, creating VM Clusters, and connecting to Amazon VPC. If done entirely on AWS Console, this process is quite complex, time-consuming, and prone to errors as the number of deployment environments increases.

In this article, we will learn how to use Terraform to automate the entire Oracle Database@AWS deployment process. Through the Infrastructure as Code (IaC) model, businesses can standardize infrastructure, reduce configuration errors, and deploy systems quickly and consistently.

---

## Architecture Guidance

Oracle Database@AWS is designed to combine Oracle Exadata's performance with AWS's scalability and flexibility. Instead of operating Oracle Database on separate infrastructure, businesses can deploy directly on AWS infrastructure while still using Exadata's optimized features.

In this model, Terraform acts as the tool orchestrating the entire resource creation process. Infrastructure components are described using HCL source code and automatically deployed by Terraform in the correct dependency order, helping ensure consistency across environments.

The solution includes four main components:

* ODB Network: A dedicated network connecting AWS and Oracle Cloud Infrastructure (OCI), providing low-latency private connectivity.
* Oracle Exadata Infrastructure: Exadata hardware infrastructure deployed directly in AWS Availability Zone to provide high-performance data processing capabilities.
* Exadata VM Cluster: Virtual machine cluster running Oracle Grid Infrastructure and Oracle Database.
* ODB Peering Connection: Private network connection between Amazon VPC and ODB Network, allowing applications on AWS to securely access Oracle Database.

> _Figure 1. Overall architecture; colored boxes represent distinct services._

After successfully creating the above components, businesses can deploy Oracle Database with full scalability, high availability, and integration capabilities with AWS services.

---

## Why Use Terraform?

Terraform is one of the most popular Infrastructure as Code tools today. Instead of operating directly on the AWS management interface, the entire infrastructure is described in HCL (HashiCorp Configuration Language) source code. Terraform reads these configuration files and automatically creates resources in the correct dependency order.

Using Terraform provides many benefits:

* Complete automation of infrastructure deployment.
* Elimination of manual configuration operations on AWS Console.
* Ensures consistent configuration across Development, Testing, and Production environments.
* Easy to manage changes through Git.
* Configurations can be reused for multiple different projects.
* Supports infrastructure scaling and updates without affecting existing resources.

For organizations deploying multiple Oracle Database systems or adopting DevOps practices, Terraform helps significantly reduce deployment time and increase infrastructure control.

---

## Prerequisites Before Deployment

Before running Terraform, ensure the following preparation steps are completed:

### 1. Register Oracle Database@AWS

Businesses need to register Oracle Database@AWS service through AWS Marketplace and complete the Onboarding process with Oracle.

### 2. Link Accounts

AWS Account must be linked with Oracle Cloud Infrastructure (OCI Tenancy) so Terraform has permission to manage resources on both platforms.

Configure Terraform Provider

Terraform needs to configure two Providers simultaneously:

* AWS Provider
* OCI Provider

These two Providers allow Terraform to communicate with AWS and Oracle Cloud APIs.

### 3. Set Up IAM Permissions

The account used by Terraform needs full permissions to create, update, and delete resources on AWS as well as Oracle Cloud.

After completing the above conditions, administrators can begin deploying Oracle Database@AWS infrastructure with Terraform.

---

## Terraform Deployment Process

Terraform will create Oracle Database@AWS components in the correct dependency order. Each component is defined as a Resource in Terraform.

### Step 1. Initialize ODB Network

The first step is to create Oracle Database Network, which establishes a private network connection between AWS and Oracle Cloud Infrastructure.

Example Terraform configuration:

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

#### In the above configuration:

* display_name defines the name of the ODB Network.
* availability_zone_id specifies the Availability Zone for deployment.
* client_subnet_cidr and backup_subnet_cidr define two network ranges for application traffic and backup.
* s3_access allows Oracle Database to access Amazon S3.
* zero_etl_access enables Zero-ETL integration capability.

After completing this step, all Oracle Database resources will be deployed on the newly created ODB Network.

### Step 2. Deploy Oracle Exadata Infrastructure

Next, Terraform initializes Exadata hardware infrastructure.

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

Oracle Exadata Infrastructure provides dedicated hardware platform for operating Oracle Database with high performance.

At this step, administrators can choose configurations suitable for their needs such as:

* Compute Node
* Storage Server
* Storage capacity
* Availability Zone

Terraform will automatically link Exadata Infrastructure with the ODB Network created in the previous step.

### Step 3. Initialize Exadata VM Cluster

After Exadata Infrastructure is ready, Terraform continues to deploy VM Cluster.

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

VM Cluster is the environment that directly runs Oracle Database.

Here, administrators can configure:

* Number of CPU Cores.
* Memory.
* Oracle Grid Infrastructure.
* Oracle Home.
* Oracle Database version.

With Terraform, all these parameters are stored as source code, making it easy to scale or reuse for multiple different environments.

### Step 4. Set Up ODB Peering Connection

After Oracle Database Infrastructure is created, the final step is to establish network connection between Amazon VPC and ODB Network.

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

Peering Connection allows applications deployed on Amazon EC2, Amazon ECS, or Amazon EKS to access Oracle Database through private network connection, reducing latency and enhancing security.

After Terraform completes this step, the entire Oracle Database@AWS infrastructure is ready to deploy and operate the database.

---

## Architecture After Deployment

After Terraform completes the provisioning process, the entire Oracle Database@AWS infrastructure will be created in the correct dependency order. Terraform automatically manages relationships between resources, so administrators don't need to perform manual operations on AWS Console.

The architecture after deployment includes the following components:

| Component                    | Role                                                                 |
| ----------------------------- | ----------------------------------------------------------------------- |
| ODB Network                   | Establishes private network connecting AWS with Oracle Cloud Infrastructure (OCI). |
| Oracle Exadata Infrastructure | Provides Exadata hardware infrastructure serving Oracle Database.             |
| Oracle Exadata VM Cluster     | Environment running Oracle Grid Infrastructure and Oracle Database.          |
| ODB Peering Connection        | Connects Amazon VPC with ODB Network for application Database access.       |
| AWS Provider                  | Manages and deploys resources on AWS.                              |
| OCI Provider                  | Manages Oracle Cloud Infrastructure resources.                         |

All resources are managed by Terraform in **Terraform State**, making it easy to track infrastructure status, update, or extend the system in the future.

---

## Deploying Project with Terraform

AWS has provided a sample project on GitHub to help users quickly deploy Oracle Database@AWS with Terraform.

First, download the source code:

```bash
git clone https://github.com/aws-samples/sample-odb-launch-using-terraform.git

cd sample-odb-launch-using-terraform
```

After downloading the source code, initialize Terraform:

```bash
terraform init
```

The above command will download necessary Providers such as AWS Provider and OCI Provider.

Next, check the deployment plan:

```bash
terraform plan
```

Terraform will analyze all source code and display the list of resources to be created, updated, or deleted.

If all configurations are correct, proceed with deployment:

```bash
terraform apply
```

Terraform will sequentially execute:

1. Create ODB Network.
2. Initialize Oracle Exadata Infrastructure.
3. Create Oracle Exadata VM Cluster.
4. Set up ODB Peering Connection.
5. Update Terraform State.

After completion, the Oracle Database@AWS environment is ready to deploy Database and connect with applications on AWS.

---

## Best Practices When Using Terraform

For Oracle Database systems in enterprise environments, AWS recommends applying some best practices to increase stability and manageability.

### 1. Always run `terraform plan` before `terraform apply`

Checking the deployment plan helps detect unexpected changes early before affecting the Production environment.

### 2. Manage Terraform State Remotely

Do not store `terraform.tfstate` file on personal computers.

Instead, use:

* Amazon S3 to store State.
* Amazon DynamoDB for State Locking.

This allows multiple team members to work simultaneously without conflicts.

### 3. Manage Source Code with Git

All Terraform code should be stored on GitHub or GitLab for:

* Tracking change history.
* Performing Code Review.
* Restoring configuration when needed.
* CI/CD integration.

### 4. Separate Environments

Use variables or Terraform Workspaces to separate environments:

* Development
* Testing
* Staging
* Production

This helps limit impacts between environments and simplifies the deployment process.

### 5. Limit Direct Changes on AWS Console

After applying Infrastructure as Code, all changes should be made through Terraform instead of editing directly on AWS Console. This helps avoid **Configuration Drift**, ensuring actual infrastructure is always consistent with source code.

---

## Target Audience

Oracle Database@AWS combined with Terraform solution is suitable for various technical teams.

* **Database Administrator (DBA):** Automate Oracle Database deployment and administration process.
* **Cloud Engineer:** Standardize Oracle infrastructure on AWS using Infrastructure as Code model.
* **DevOps Engineer:** Integrate database deployment into CI/CD process.
* **Enterprise Architect:** Design Oracle Database infrastructure with scalability and easy maintenance.
* **Enterprise Business:** Organizations transitioning Oracle Database systems from On-premises to AWS while still requiring Oracle Exadata's high performance.

---

## Conclusion

Oracle Database@AWS brings a new approach to deploying Oracle Database on cloud platforms by combining Oracle Exadata's performance with AWS's service ecosystem. However, along with that comes the configuration process for multiple infrastructure components and consistency requirements during deployment.

Terraform helps solve this problem through the **Infrastructure as Code** model, allowing the entire infrastructure to be defined in source code instead of manual operations on the management interface. Thanks to automation, version management, and configuration reuse capabilities, Terraform not only shortens deployment time but also reduces risks from configuration errors and improves system operation efficiency.

For businesses building or modernizing Oracle Database infrastructure on AWS, combining Oracle Database@AWS with Terraform is an appropriate solution to standardize deployment processes, increase scalability, and simplify long-term administration.

Source: AWS Database Blog - Provision Oracle Database@AWS resources using Terraform.
