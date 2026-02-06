# Basic AWS Infrastructure with Terraform

This project deploys a simple AWS infrastructure using Terraform. It includes a VPC, a public subnet, an internet gateway, a security group (allowing SSH and HTTP), and a single EC2 instance (Ubuntu).

## Prerequisites

1.  **Terraform**: Install Terraform from [terraform.io](https://www.terraform.io/downloads).
2.  **AWS Account**: You need an active AWS account.
3.  **AWS Credentials**: Configure your AWS credentials using `aws configure` (requires AWS CLI) or by setting environment variables:
    *   `AWS_ACCESS_KEY_ID`
    *   `AWS_SECRET_ACCESS_KEY`
    *   `AWS_REGION` (default is `us-east-1` in this setup, can be overridden)
4.  **SSH Key Pair**: Ensure you have an EC2 Key Pair created in your AWS region (default `us-east-1`) named `my-key-pair`. If your key has a different name, override it using a `.tfvars` file or command line argument.

## Usage

### 1. Initialize Terraform

Initialize the working directory containing Terraform configuration files.

```bash
terraform init
```

### 2. Plan the Deployment

Preview the changes that Terraform will make. This step is optional but recommended.

```bash
terraform plan
```

If you need to customize variables (e.g., key pair name):

```bash
terraform plan -var="key_name=your-key-pair-name"
```

### 3. Apply the Configuration

Create the infrastructure on AWS.

```bash
terraform apply
```

Type `yes` when prompted to confirm.

After successful application, Terraform will output the connection details:

```
Outputs:

instance_public_dns = "ec2-x-x-x-x.compute-1.amazonaws.com"
instance_public_ip = "x.x.x.x"
ssh_connection_string = "ssh -i my-key-pair.pem ubuntu@x.x.x.x"
```

### 4. Destroy the Infrastructure

To remove all resources created by this configuration and avoid further costs:

```bash
terraform destroy
```

Type `yes` when prompted to confirm.

## Files

*   `main.tf`: Defines the core resources (VPC, Subnet, SG, EC2).
*   `variables.tf`: Defines input variables (`aws_region`, `instance_type`, `key_name`).
*   `outputs.tf`: Defines the output values returned after deployment.
*   `provider.tf`: Configures the AWS provider.

## Notes

*   **Security**: The Security Group is configured to allow SSH (`0.0.0.0/0`) and HTTP (`0.0.0.0/0`) from anywhere. **For production, you should restrict the SSH access to your specific IP address.**
*   **Cost**: This setup uses `t2.micro` which is eligible for the AWS Free Tier (if you qualify).
