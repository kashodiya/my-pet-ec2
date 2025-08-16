# My Pet EC2

This project creates an EC2 server on AWS for development and experimentation purposes. It sets up a complete environment with necessary security groups, IAM roles, and networking configurations.

## Prerequisites

- Windows machine for running the scripts
- AWS CLI installed and configured with appropriate credentials
- Existing VPC and subnet with Internet access
- Terraform v1.9.0 or higher

## Overall Design

- Uses Terraform to create an EC2 instance and all related infrastructure on AWS
- Keeps the design simple to minimize costs
- Installs all required tools and dependencies on a single EC2 server
- Includes OpenHands integration with LiteLLM and VSCode

## Infrastructure Components

- **EC2 Instance**: Amazon Linux 2 AMI on m5.xlarge with 80GB storage
- **Security Group**: Configured for SSH (port 22), HTTP (port 80), application ports (3000-7000), and OpenHands ports (30000-60000)
- **IAM Role**: With permissions for SSM, CloudWatch, and AWS Bedrock
- **Elastic IP**: For consistent access to the instance
- **Key Pair**: Automatically generated for SSH access

## Getting Started

1. Clone this repository
2. Navigate to the terraform directory
3. Copy `terraform.tfvars.example` to `terraform.tfvars` and update with your values:
   - `subnet_id`: Your subnet ID (the VPC ID will be automatically determined from this)
   - `allowed_ips`: List of IP addresses allowed to access the EC2 instance
   - `openhands_litellm_key`: Your LiteLLM API key
   - `openhands_vscode_token`: Your VSCode connection token

4. Run `terraform init` followed by `terraform apply`

## Included Scripts

- **ssh-ec2.bat**: Connect to the EC2 instance via SSH
- **recreate-ec2.bat**: Recreate the EC2 instance while preserving other resources
- **remove-host.bat**: Remove the SSH host key for the EC2 instance
- **tail-logs.bat**: View the EC2 instance's user data logs

## Security Considerations

- Access is restricted to specified IP addresses
- SSH key is automatically generated and stored locally
- Sensitive information is stored in AWS SSM Parameter Store

## Important Notes

- The project references a `user-data.sh` file in the Terraform configuration that should be placed in the terraform directory. This file is not included in the repository and needs to be created before running `terraform apply`.
- The `user-data.sh` file should contain the initialization script that will run when the EC2 instance is launched.

## OpenHands Integration

This project includes integration with OpenHands:

- Configures ports 30000-60000 for OpenHands services
- Stores LiteLLM API key and VSCode connection token in AWS SSM Parameter Store
- Provides access to AWS Bedrock for AI model invocation
- The EC2 instance is configured with permissions to access these services

## Outputs

After running Terraform, the following outputs are available:
- Elastic IP address
- Instance ID
- Security Group ID
- Key Pair name
- Subnet ID
- VPC ID

These values are saved to `outputs.env` in the terraform directory. This file is critical as it's used by all the utility scripts:
- `ssh-ec2.bat` uses the Elastic IP to connect to the instance
- `remove-host.bat` uses the Elastic IP to remove SSH host keys
- `tail-logs.bat` uses the Elastic IP to connect and tail logs
- `recreate-ec2.bat` preserves these outputs when recreating the instance

Make sure the `outputs.env` file exists before running any of the scripts. This file is created after the first successful `terraform apply` and contains the necessary information for the scripts to work properly.

## Troubleshooting

- **Cannot connect to EC2**: Ensure your IP address is in the `allowed_ips` list and that the subnet has internet access
- **Missing user-data.sh**: Create this file in the terraform directory before running `terraform apply`
- **SSH key issues**: Use the `remove-host.bat` script if you get SSH host key verification errors
- **Instance logs**: Use the `tail-logs.bat` script to view the EC2 instance's initialization logs
