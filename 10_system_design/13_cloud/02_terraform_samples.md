# 02_terraform_samples.md


## Objective / 目的

- Provides Terraform configuration examples for AWS infrastructure components such as VPC, EC2, and S3.  
- Understand how to automate cloud environment deployment using Infrastructure as Code (IaC).

- VPC・EC2・S3といったAWSインフラ構成要素をTerraformで自動化する実践的な例を示します。  
- Infrastructure as Code（IaC）を用いたクラウド環境構築を理解することを目的としています。

---

## Scenario / シナリオ

- Your organization is expanding its internal systems to the cloud.  
- As part of this migration, you are tasked with defining AWS infrastructure using Terraform to ensure repeatable and secure deployments.  
- You will create a VPC, deploy an EC2 instance inside it, and store logs or backup data in S3 — all automated through code.

- 自社システムのクラウド移行を進める中で、再現性とセキュリティを確保した環境構築が求められています。  
- その一環として、Terraformを使用してAWS上のインフラをコードで定義し、自動的に構築します。  
- VPCを作成し、その中にEC2インスタンスを配置し、ログやバックアップ用にS3を設定する構成を例に示します。

---
## File Summary / ファイル表

1. vpc.tf
2. ec2.tf
3. s3.tf

---
## 1. vpc.tf

- VPC & Network Configuration / VPC及びネットワーク構成サンプル
```bash
# ----
# VPC = Tokyo region / VPCに東京リージョンを指定
# ----

provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_vpc" "main_vpc" {
  cidr_block = "10.0.0.0/16"
  instance_tenancy = "default"
  enable_dns_support   = true
  enable_dns_hostnames = true
  assign_generated_ipv6_cidr_blocks = false
  
  tags = {
    Name = "main-vpc"
  }
}

# ----
# Subnet / サブネット
# ----

resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.main_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "ap-northeast-1a"
  tags = {
    Name = "public-subnet"
  }
}

# ----
# Internet Gateway / インターネットゲートウエイ
# ----

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id
  tags = {
    Name = "main-igw"
  }
}

# ----
# Route Table / ルートテーブル
# ----

resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
  tags = {
    Name = "public-rt"
  }
}

# ----
# Routing Association / ルートテーブル依存関係を明示
# ----


resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

---
## 2. ec2.tf

- EC2 Instance Configuration / EC2インスタンス構成サンプル

```bash
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_security_group" "ec2_sg" {
  name        = "ec2-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.main_vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "ec2-sg"
  }
}

resource "aws_instance" "web_server" {
  ami           = "ami-0c3fd0f5d33134a76"  # Amazon Linux 2
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public_subnet.id
  security_groups = [aws_security_group.ec2_sg.name]
  key_name      = "my-keypair"

  tags = {
    Name = "web-server"
  }
}
```

---
## 3. s3.tf

- S3 Bucket Configuration / S3バケット構成サンプル
```bash
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_s3_bucket" "logs_bucket" {
  bucket = "company-logs-demo-bucket"
  acl    = "private"

  versioning {
    enabled = true
  }

  server_side_encryption_configuration {
    rule {
      apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
      }
    }
  }

  tags = {
    Name        = "logs-bucket"
    Environment = "dev"
  }
}
```

---
## Note / 補足
 
- Apply changes along with these commands / 以下のコマンドを適宜使用して設定を適用
```bash
# Initialization / 初期化
terraform init

#Formatting / フォーマットを整える
terraform fmt

#Check configuration / 設定内容を確認
terraform plan

#Apply changes / 設定を適用
terraform apply -auto-approve
```

---
## Reference
- You  can also check ->
	- < Udemy > AWS と Terraformで実現するInfrastructure as Code by 津郷 晶也 https://www.udemy.com/course/iac-with-terraform/
