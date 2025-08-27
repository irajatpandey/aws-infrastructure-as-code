# Terraform AWS Baseline

A comprehensive, production-ready AWS infrastructure baseline built with Terraform.

---

## 🚀 Project Overview

This repository provides a modular and secure foundation for deploying applications on AWS. It's designed to showcase a mature Infrastructure as Code (IaC) workflow, addressing key real-world challenges such as state management, security, cost control, and infrastructure drift.

The project leverages best practices to create a scalable and maintainable infrastructure, making it an excellent starting point for new projects or a solid reference for existing ones.

## 📦 What's Included

The core infrastructure components are built as reusable Terraform modules to promote DRY (Don't Repeat Yourself) principles.

- **VPC & Networking:** A highly available VPC with public, private, and database subnets, configured with an Internet Gateway and NAT Gateway.
- **EKS Cluster:** A production-grade Elastic Kubernetes Service (EKS) cluster with managed node groups and all necessary IAM roles.
- **RDS Database:** A secure, multi-AZ RDS database instance (PostgreSQL) deployed within a private subnet.
- **IAM Roles & Policies:** Granular IAM configurations for service accounts and applications, adhering to the principle of least privilege.

## ✨ Key Features & Best Practices

- **Remote State Management:** Configured with an S3 backend and DynamoDB table for state locking, preventing concurrent modifications and ensuring state integrity.
- **CI/CD Workflow:** A GitHub Actions pipeline that automates:
    - `terraform validate` and `tflint` for code quality checks.
    - `terraform plan` on every pull request to preview changes.
    - `terraform apply` on merge to `main` for automated deployments.
- **Policy-as-Code:** Integration with security scanning tools like `trivy` or `checkov` to enforce security policies and best practices at build time, not runtime.
- **Cost Consciousness:** Includes a detailed section on cost estimation and notes on using AWS tags for effective cost allocation and monitoring.
- **Drift Handling:** A documented strategy for detecting and mitigating infrastructure drift, ensuring the deployed state matches the version-controlled code.

## 🛠️ How to Use

### Prerequisites

- [Terraform CLI](https://developer.hashicorp.com/terraform/downloads) (v1.0+)
- [AWS CLI](https://aws.amazon.com/cli/) configured with the necessary credentials
- A dedicated S3 bucket and DynamoDB table for Terraform state (see `backend.tf` for details)

### Workflow

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/your-username/terraform-aws-baseline.git](https://github.com/your-username/terraform-aws-baseline.git)
    cd terraform-aws-baseline
    ```
2.  **Initialize Terraform:**
    ```bash
    terraform init
    ```
3.  **Review the Plan:**
    ```bash
    terraform plan
    ```
4.  **Apply the Configuration:**
    ```bash
    # WARNING: This will create resources in your AWS account and incur costs.
    terraform apply
    ```

## 💰 Cost Notes

The infrastructure deployed by this repository will incur AWS costs. Below is a high-level estimate of the main cost drivers:

- **EKS Control Plane:** Approx. $72/month.
- **EC2 Instances (EKS Node Group):** Varies based on instance type and usage. A small `t3.medium` instance might cost around $30/month.
- **NAT Gateway:** A significant cost driver, approx. $32.40/month per gateway + data transfer charges.
- **RDS Instance:** Varies based on instance size and storage. A `db.t3.micro` is typically free tier eligible, but a `db.t3.small` can be around $15-20/month.

**Recommendation:** Utilize AWS cost allocation tags (e.g., `Project=my-app`, `Owner=your-name`) to track and manage costs effectively.

## 🚦 Infrastructure Drift

Infrastructure drift occurs when the state of your cloud resources deviates from your Terraform code. This repository's workflow mitigates this by:

1.  **Centralized State:** Storing state remotely prevents manual modifications from being overwritten.
2.  **CI/CD Pipeline:** Enforces that all changes go through the CI/CD pipeline, ensuring a single source of truth.
3.  **Periodic Checks:** It is highly recommended to run `terraform plan` periodically (e.g., via a scheduled job) to detect and alert on any manual changes made in the AWS console.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
