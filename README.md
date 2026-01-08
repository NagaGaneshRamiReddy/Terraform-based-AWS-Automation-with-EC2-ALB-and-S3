# Terraform-based-AWS-Automation-with-EC2-ALB-and-S3
Automated AWS infrastructure deployment using Terraform, including EC2, ALB, and S3, with secure networking and scalable architecture.
Project Documentation: Terraform AWS Infrastructure
Project Name

Terraform AWS Infrastructure Automation

Project Overview

This project demonstrates the deployment of real AWS infrastructure using Terraform, including:

EC2 instances with a running Apache web server

Application Load Balancer (ALB) distributing traffic across multiple EC2s

S3 bucket for storage

Proper security groups and VPC networking

Fully automated provisioning via Terraform (Infrastructure as Code)

Objective:
Build a scalable, automated, and verifiable AWS infrastructure project suitable for DevOps, Cloud, or SRE roles.

Technologies & Tools Used
Technology	Purpose
Terraform	Infrastructure as Code (IaC)
AWS EC2	Virtual servers
AWS S3	Storage bucket
AWS ALB	Load balancing HTTP traffic
Security Groups	Network security
VPC & Subnets	Networking & isolation
Git/GitHub	Version control & portfolio
Architecture Diagram
         Internet
            |
            |
   [Application Load Balancer]
            |
       -----------------
       |               |
 [EC2 Instance 1]   [EC2 Instance 2]
       |
   Apache Web Server


ALB distributes HTTP traffic across 2 EC2 instances.

S3 bucket serves as storage for project data or logs.

Terraform Project Structure
terraform-aws-project/
├── main.tf          # Core resources: EC2, ALB, S3, SGs
├── variables.tf     # Input variables
├── outputs.tf       # Output resources for verification
├── terraform.tfvars # Optional variable values
├── README.md        # Documentation
└── .gitignore       # Ignore sensitive/state files

Terraform Resources Explained
1. Provider
provider "aws" {
  region = var.region
}

2. EC2 Instances

Count variable allows scaling

User data installs Apache and serves a basic web page

resource "aws_instance" "ec2" {
  count                      = var.instance_count
  ami                        = var.ami
  instance_type              = var.instance_type
  associate_public_ip_address = true
  vpc_security_group_ids     = [aws_security_group.ec2_sg.id]

  user_data = <<-EOF
              #!/bin/bash
              yum install -y httpd
              systemctl start httpd
              systemctl enable httpd
              echo "Hello from EC2 ${count.index + 1}" > /var/www/html/index.html
              EOF

  tags = {
    Name = "r.n.g-${count.index + 1}"
  }
}

3. S3 Bucket
resource "aws_s3_bucket" "my_bucket" {
  bucket = var.s3_bucket_name

  tags = {
    Name        = "Terraform S3 Bucket"
    Environment = "Dev"
  }
}

4. Application Load Balancer
resource "aws_lb" "alb" {
  name               = "r-n-g-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb_sg.id]
  subnets            = data.aws_subnets.default.ids
}

resource "aws_lb_target_group" "tg" {
  name     = "r-n-g-tg"
  port     = 80
  protocol = "HTTP"
  vpc_id   = data.aws_vpc.default.id
}

resource "aws_lb_listener" "listener" {
  load_balancer_arn = aws_lb.alb.arn
  port              = 80
  protocol          = "HTTP"

  default_action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.tg.arn
  }
}

resource "aws_lb_target_group_attachment" "attach" {
  count            = var.instance_count
  target_group_arn = aws_lb_target_group.tg.arn
  target_id        = aws_instance.ec2[count.index].id
  port             = 80
}

5. Security Groups

ALB SG → allows HTTP from anywhere

EC2 SG → allows HTTP only from ALB SG

resource "aws_security_group" "alb_sg" {
  ...
}

resource "aws_security_group" "ec2_sg" {
  ...
}

6. Outputs
output "alb_dns" {
  value = aws_lb.alb.dns_name
}

output "ec2_public_ips" {
  value = aws_instance.ec2[*].public_ip
}

output "s3_bucket_name" {
  value = aws_s3_bucket.my_bucket.bucket
}

Setup Instructions

Clone the repo:

git clone https://github.com/<username>/terraform-aws-project.git
cd terraform-aws-project


Initialize Terraform:

terraform init


Plan and apply:

terraform plan
terraform apply


Type yes when prompted.

Access the ALB DNS in browser to see load-balanced web pages.
