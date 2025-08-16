# My Pet EC2
This project creates a EC2 server. Purpose of this. EC2 server is to create a development environment for experimentations. 

## Assumptions
- This project will be run from a Windows machine
- User has already authenticated to AWS and it's credentials are set either as an environment, variable or a profile
- User has already created a VPC and a subnet which can access Internet

## Overall Design 
- Use terraform to create EC2 an all related infrastructure on AWS
- Keep the design very simple so we reduce the cost
- Everything that we need will be install installed on single EC2 server

## Terraform design
- This project allows user to specify things like VPC ID, Subnet ID, list of IP addresses allowed to connect to this EC2 etc., using a tfvar file.
- Terraform also creates a key to be used for EC2
