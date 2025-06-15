# AWS Infrastructure Deployment with Terraform and CloudFormation

Dự án này triển khai cơ sở hạ tầng AWS sử dụng cả Terraform và CloudFormation, với CI/CD pipeline thông qua GitHub Actions và AWS CodePipeline.

## Cấu trúc dự án

```
.
├── infrastructure/           # Terraform configuration
│   ├── main.tf              # Main Terraform configuration
│   ├── variables.tf         # Terraform variables
│   ├── outputs.tf           # Terraform outputs
│   └── eks.tf              # EKS cluster configuration
├── .github/workflows/       # GitHub Actions workflows
│   ├── deploy.yml          # Terraform deployment workflow
│   ├── destroy.yml         # Terraform destroy workflow
│   ├── cfn-deploy.yml      # CloudFormation deployment workflow
│   ├── cfn-destroy.yml     # CloudFormation destroy workflow
│   ├── setup-backend.yml   # S3 & DynamoDB setup workflow
│   └── destroy-backend.yml # Backend cleanup workflow
├── codepipeline.yaml       # AWS CodePipeline configuration
├── infrastructure.yaml     # CloudFormation template
├── buildspec.yml          # AWS CodeBuild specification
└── .taskcat.yml           # TaskCat configuration
```

## Các thành phần chính

### 1. Terraform Infrastructure

- VPC với public/private subnets
- EKS cluster với managed node groups
- S3 backend cho state management
- DynamoDB cho state locking

### 2. CloudFormation Infrastructure

- VPC với public/private subnets
- EC2 instance với security groups
- NAT Gateway
- Internet Gateway

### 3. CI/CD Pipelines

- GitHub Actions workflows cho Terraform và CloudFormation
- AWS CodePipeline với CodeBuild integration

## Yêu cầu

- AWS CLI
- Terraform >= 1.5.0
- Python 3.11 (cho CodeBuild)
- AWS Account với quyền admin
- GitHub repository

## Cài đặt và Sử dụng

# Triển khai hạ tầng AWS sử dụng Terraform và tự động hóa quy trình với GitHub Actions

### 1. Thiết lập Backend

```bash
# Chạy GitHub Action để tạo S3 bucket và DynamoDB table
# Truy cập GitHub repository -> Actions -> setup-backend.yml -> Run workflow
```

### 2. Triển khai Terraform

```bash
# Chạy GitHub Action để triển khai Terraform
# Truy cập GitHub repository -> Actions -> deploy.yml -> Run workflow
```

### 3. Triển khai CloudFormation

```bash
# Chạy GitHub Action để triển khai CloudFormation
# Truy cập GitHub repository -> Actions -> cfn-deploy.yml -> Run workflow
```

# Triển khai hạ tầng AWS với CloudFormation và tự động hóa quy trình build và deploy với AWS CodePipeline

### 4. Thiết lập CodePipeline

```bash
# Deploy CodePipeline stack
aws cloudformation deploy `
  --template-file codepipeline.yaml `
  --stack-name my-cfn-codepipeline `
  --capabilities CAPABILITY_NAMED_IAM `
  --parameter-overrides RepoName=your-codecommit-repo
```

## Xóa Infrastructure

### 1. Xóa CloudFormation Stack

```bash
# Chạy GitHub Action để xóa CloudFormation stack
# Truy cập GitHub repository -> Actions -> cfn-destroy.yml -> Run workflow
```

### 2. Xóa Terraform Infrastructure

```bash
# Chạy GitHub Action để xóa Terraform infrastructure
# Truy cập GitHub repository -> Actions -> destroy.yml -> Run workflow
```

### 3. Xóa Backend Resources

```bash
# Chạy GitHub Action để xóa S3 bucket và DynamoDB table
# Truy cập GitHub repository -> Actions -> destroy-backend.yml -> Run workflow
```

## Lưu ý

1. Đảm bảo đã cấu hình AWS credentials trong GitHub Secrets:

   - AWS_ACCESS_KEY_ID
   - AWS_SECRET_ACCESS_KEY

2. Các IAM roles trong template đang sử dụng AdministratorAccess policy. Trong môi trường production, nên giảm quyền xuống mức tối thiểu cần thiết.

3. Một số resources như AMI ID và region được hardcode. Nên parameterize chúng trong môi trường production.

## Bảo mật

- Không commit AWS credentials vào repository
- Sử dụng IAM roles với quyền tối thiểu cần thiết
- Enable versioning cho S3 buckets
- Sử dụng private subnets cho các resources nhạy cảm
- Enable encryption cho các resources khi có thể

## Contributing

1. Fork repository
2. Tạo feature branch
3. Commit changes
4. Push to branch
5. Tạo Pull Request

## License

MIT
