Terraform AWS Infrastructure Project
------------------------------------------
Project
---------

Automated deployment of AWS infrastructure using Terraform, including EC2 instances, an Application Load Balancer (ALB), and an S3 bucket. The project demonstrates Infrastructure as Code (IaC), scalable architecture, and secure cloud setup.

problem
---------
Manually deploying cloud infrastructure is:

Time-consuming

Error-prone

Hard to scale and reproduce

Difficult to maintain consistently across environments

Companies need automated, scalable, and reproducible infrastructure for cloud applications.

Solution / Development
---------------------

This project uses Terraform to automate AWS infrastructure deployment:

EC2 Instances: Two servers hosting a simple web page

Application Load Balancer (ALB): Distributes traffic across EC2 instances for high availability

S3 Bucket: Provides secure storage for files

Security Groups & VPC: Ensures secure networking and controlled access

Terraform Variables & Outputs: Makes the infrastructure configurable and outputs useful information (ALB URL, EC2 IPs, S3 bucket name)

Workflow:
--------------

Define cloud resources in Terraform files (main.tf, variables.tf, outputs.tf)

Initialize Terraform to download AWS provider

Apply the configuration to automatically provision resources

Access the ALB DNS to verify deployment

Benefits:
-------------

Fully automated, reproducible, and scalable infrastructure

Secure and modular design using Terraform

Reduces manual effort and human error

Terraform Commands
# Initialize Terraform (download providers)
terraform init

# Show planned resources before deployment
terraform plan

# Deploy infrastructure
terraform apply
# Type 'yes' when prompted

# Destroy infrastructure when no longer needed
terraform destroy
# Type 'yes' when prompted
