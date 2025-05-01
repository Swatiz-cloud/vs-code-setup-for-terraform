# 🚀 Terraform + AWS Integration with Visual Studio Code

This guide explains how to integrate **Terraform** with your **AWS account** using **Visual Studio Code (VS Code)** for seamless infrastructure development and deployment.



## 🧰 Prerequisites

Before you begin, ensure you have:

- ✅ An AWS account
- ✅ Installed [Visual Studio Code](https://code.visualstudio.com/)
- ✅ Installed [Terraform](https://developer.hashicorp.com/terraform/downloads)
- ✅ Installed [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
- ✅ Created an **IAM user** in AWS with **programmatic access** and necessary permissions (like `AmazonEC2FullAccess`)



## ⚙️ Step 1: Set Up AWS Credentials

1. Open a terminal (within VS Code or OS terminal).
2. Run:

   ```bash
   aws configure
   ```

3. Enter your:
   - AWS Access Key ID
   - AWS Secret Access Key
   - Default region (e.g., `us-east-1`)
   - Output format (e.g., `json`)

Credentials will be saved to:
- `~/.aws/credentials`
- `~/.aws/config`



## 🧩 Step 2: Install VS Code Extensions

In VS Code, go to **Extensions (Ctrl+Shift+X)** and install:

- ✅ **Terraform** by HashiCorp
- ✅ **AWS Toolkit** (optional, for AWS integration)
- ✅ **Prettier** (optional, for code formatting)



## 📁 Step 3: Create a Terraform Project


1. Create a new folder:

   ```bash
   mkdir terraform-aws-demo && cd terraform-aws-demo
   ```


2. Create these files:

```
terraform-aws-demo/
├── main.tf
├── variables.tf
├── terraform.tfvars
├── outputs.tf
```


### 📝 main.tf

```hcl
provider "aws" {
  region = var.region
}

resource "aws_instance" "demo_ec2" {
  ami           = var.ami
  instance_type = var.instance_type
  key_name      = var.key_name

  vpc_security_group_ids = [aws_security_group.allow_ssh.id]

  tags = {
    Name = "VSCodeEC2Instance"
  }
}

resource "aws_security_group" "allow_ssh" {
  name        = "allow_ssh"
  description = "Allow SSH from anywhere"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```


### 📘 variables.tf

```hcl
variable "region" {}
variable "ami" {}
variable "instance_type" {}
variable "key_name" {}
```


### 📦 terraform.tfvars

```hcl
region         = "us-east-1"
ami            = "ami-0c02fb55956c7d316"
instance_type  = "t2.micro"
key_name       = "your-key-name"  # Replace with your existing AWS EC2 Key Pair name
```


### 📤 outputs.tf

```hcl
output "instance_ip" {
  value = aws_instance.demo_ec2.public_ip
}
```



## 🧪 Step 4: Run Terraform Commands in VS Code Terminal

1. Initialize the project:

   ```bash
   terraform init
   ```

2. Review the plan:

   ```bash
   terraform plan
   ```

3. Apply and launch resources:

   ```bash
   terraform apply -auto-approve
   ```

4. Get the public IP of the instance:

   ```bash
   terraform output instance_ip
   ```



## 🔐 Connect to EC2 Instance (Optional)

Use the terminal:

```bash
ssh -i your-key-name.pem ec2-user@<instance_ip>
```



## 🧹 Cleanup

To avoid charges:

```bash
terraform destroy -auto-approve
```



## ✅ Summary

You have successfully:

- Installed and configured Terraform in VS Code
- Integrated it with your AWS credentials
- Provisioned an EC2 instance using Infrastructure as Code



## 📎 Useful Resources

- [Terraform AWS Provider Docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [Terraform Learn](https://learn.hashicorp.com/terraform)

