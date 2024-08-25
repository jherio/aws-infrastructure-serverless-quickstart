# AWS Infrastructure with Terraform

This project sets up a comprehensive AWS infrastructure using Terraform. The infrastructure includes:

- ECR (Elastic Container Registry)
- S3 (Simple Storage Service)
- Lambda
- API Gateway
- CloudFront
- ACM (AWS Certificate Manager)
- Route53

## Prerequisites

- AWS CLI configured with appropriate permissions
- Docker installed (for pushing images to ECR)
- Terraform installed (preferably using tfenv for version management)

## Getting Started

### Install Terraform

We recommend using tfenv for Terraform version management:

```bash
brew install tfenv
tfenv install <required_version>
tfenv use <required_version>
```

### Bootstrap the Environment

Create the initial resources:

1. S3 bucket for Terraform state files
2. Route53 hosted zone
3. ECR repository

```bash
cd terraform/bootstrap
terraform init
terraform plan
terraform apply -var-file=dev.tfvars
```

### Push Docker Image

Login to ECR

```bash
aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin <bootstrap_ecr_uri>
```

```bash
docker build -t <bootstrap_ecr_uri>/<image_name> . && docker push <bootstrap_ecr_uri>/<image_name>:<image_tag>
```

### Deploy Infrastructure

```bash
cd terraform
terraform init
terraform plan
terraform apply -var-file=dev.tfvars