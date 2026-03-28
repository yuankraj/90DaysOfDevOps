# 🚀 Day 61 – Introduction to Terraform & AWS Infrastructure

---

## 📌 Overview

Today I started my Infrastructure as Code (IaC) journey using Terraform.

I successfully:

* Installed Terraform and AWS CLI
* Configured AWS credentials
* Created an S3 bucket using Terraform
* Launched an EC2 instance
* Explored Terraform state
* Modified infrastructure
* Destroyed all resources

---

# 🧠 Task 1: Infrastructure as Code (IaC)

Infrastructure as Code (IaC) means managing infrastructure (servers, storage, networking) using code instead of manually creating them through a cloud console.

### Why IaC matters:

* Ensures consistency across environments
* Reduces human error
* Enables automation
* Supports version control

### Problems IaC solves:

* No repetitive manual setup
* Easy to recreate infrastructure
* Faster deployments

---

## ⚖️ Terraform vs Other Tools

| Tool           | Description                     |
| -------------- | ------------------------------- |
| Terraform      | Multi-cloud, declarative IaC    |
| CloudFormation | AWS-only IaC                    |
| Ansible        | Configuration management        |
| Pulumi         | IaC using programming languages |

---

## 🔑 Key Concepts

* **Declarative** → You define *what you want*, not how
* **Cloud-agnostic** → Works across AWS, Azure, GCP

---

# ⚙️ Task 2: Setup Terraform & AWS

## Verify Terraform

```bash id="t1"
terraform -version
```

## Configure AWS

```bash id="t2"
aws configure
```

Enter:

* Access Key
* Secret Key
* Region: ap-south-1
* Output: json

## Verify AWS

```bash id="t3"
aws sts get-caller-identity
```

---

# 📁 Task 3: Create S3 Bucket using Terraform

## Setup Project

```bash id="t4"
mkdir terraform-basics
cd terraform-basics
```

---

## main.tf (S3 Bucket)

```hcl id="t5"
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "terraweek-vishal-2026"
}
```

---

## Run Terraform

```bash id="t6"
terraform init
terraform plan
terraform apply
```

---

## Explanation

* `terraform init` → downloads AWS provider
* `.terraform/` → contains plugins
* `plan` → preview changes
* `apply` → creates resources

---

# 💻 Task 4: Add EC2 Instance

## Update main.tf

```hcl id="t7"
resource "aws_instance" "my_ec2" {
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t2.micro"

  tags = {
    Name = "TerraWeek-Day1"
  }
}
```

---

## Apply Changes

```bash id="t8"
terraform plan
terraform apply
```

---

## Explanation

Terraform knows:

* S3 bucket already exists (tracked in state)
* Only EC2 needs to be created

---

# 📂 Task 5: Terraform State

## Commands

```bash id="t9"
terraform show
terraform state list
terraform state show aws_s3_bucket.my_bucket
terraform state show aws_instance.my_ec2
```

---

## What State File Contains

* Resource IDs
* Configuration
* Metadata
* Dependencies

---

## Important Notes

* ❌ Never edit manually
* ❌ Never push to Git
* Contains sensitive info

---

# 🔁 Task 6: Modify & Destroy

## Modify EC2 Tag

```hcl id="t10"
tags = {
  Name = "TerraWeek-Modified"
}
```

---

## Plan Output Symbols

| Symbol | Meaning |
| ------ | ------- |
| +      | Create  |
| ~      | Update  |
| -      | Destroy |

---

## Apply Changes

```bash id="t11"
terraform apply
```

---

## Destroy Infrastructure

```bash id="t12"
terraform destroy
```

---

# 📊 Terraform Commands Summary

| Command              | Purpose            |
| -------------------- | ------------------ |
| terraform init       | Initialize project |
| terraform plan       | Preview changes    |
| terraform apply      | Create/update      |
| terraform destroy    | Delete resources   |
| terraform show       | View state         |
| terraform state list | List resources     |

---

# 📸 Screenshots

Add screenshots:

* terraform apply output
* AWS S3 bucket
* AWS EC2 instance

---

# 💡 What I Learned

* Terraform automates infrastructure
* State file tracks resources
* Infrastructure is reproducible
* No manual AWS setup needed
* Easy cleanup using destroy

---

# 🧠 Interview Answers

## What is IaC?

Managing infrastructure using code instead of manual setup.

## What is Terraform?

A declarative tool to provision infrastructure.

## What is State File?

Stores current infrastructure state.

## Why not commit state file?

Contains sensitive data.

## Plan vs Apply?

* Plan → preview
* Apply → execute

---

# 🧾 .gitignore

```txt id="t13"
.terraform/
*.tfstate
*.tfstate.backup
```

---

# ⚡ Commands Used

```bash id="t14"
terraform init
terraform plan
terraform apply
terraform show
terraform state list
terraform destroy

aws configure
aws sts get-caller-identity
```

---

# 🎯 Final Summary

Today I created AWS infrastructure using Terraform code.

I provisioned:

* S3 bucket
* EC2 instance

and destroyed everything using a single command.

This is the foundation of real DevOps automation.

---

# 🔥 Key Takeaway

Terraform = Infrastructure Automation
State = Source of Truth
Plan = Preview
Apply = Execute
Destroy = Cleanup

---

# 🚀 Status

✅ Day 61 Completed Successfully
