# AWS CloudFormation Lab 3: Custom VPC with Public and Private Subnets

## Project Overview
This CloudFormation template creates a complete AWS networking infrastructure with public and private subnets, along with EC2 instances in each subnet. The architecture follows AWS best practices for secure and scalable network design.

## Architecture
![Architecture Diagram](https://raw.githubusercontent.com/username/aws_cloudformation_lab3/main/architecture.png)


The architecture includes:
- A custom VPC with CIDR block 10.0.0.0/16
- Public subnet (10.0.1.0/24) with route to Internet Gateway
- Private subnet (10.0.2.0/24) with route to NAT Gateway
- Internet Gateway for public internet access
- NAT Gateway for private subnet internet access
- EC2 instances in both public and private subnets
- Security group allowing SSH (port 22) and HTTP (port 80) access

## Prerequisites
- AWS account with appropriate permissions
- AWS CLI installed and configured
- Knowledge of AWS CloudFormation
- SSH key pair named "roczyno" (or update the template with your key name)

## Deployment Instructions

### Using AWS Management Console
1. Open the AWS Management Console
2. Navigate to CloudFormation service
3. Click "Create stack" > "With new resources"
4. Select "Upload a template file" and upload the lab3.yaml file
5. Follow the prompts to name your stack and set any parameters
6. Review and create the stack

### Using AWS CLI
```bash
aws cloudformation create-stack \
  --stack-name my-vpc-stack \
  --template-body file://lab3.yaml \
  --capabilities CAPABILITY_IAM
```

## Resources Created
The template creates the following resources:

| Resource Type | Resource Name | Description |
|---------------|---------------|-------------|
| VPC | MyVPC | Custom VPC with CIDR 10.0.0.0/16 |
| Internet Gateway | MyInternetGateway | Provides internet access for the public subnet |
| Subnet | PublicSubnet | Public subnet with CIDR 10.0.1.0/24 |
| Subnet | PrivateSubnet | Private subnet with CIDR 10.0.2.0/24 |
| Route Table | PublicRouteTable | Route table for public subnet |
| Route Table | PrivateRouteTable | Route table for private subnet |
| NAT Gateway | MyNATGateway | Allows private subnet resources to access the internet |
| Security Group | MySecurityGroup | Allows SSH and HTTP traffic |
| EC2 Instance | PublicEC2Instance | t2.micro instance in public subnet |
| EC2 Instance | PrivateEC2Instance | t2.micro instance in private subnet |

## Usage Instructions

### Accessing the Public EC2 Instance
You can SSH into the public EC2 instance using:
```bash
ssh -i /path/to/roczyno.pem ec2-user@<public-ip-address>
```

### Accessing the Private EC2 Instance
The private EC2 instance is not directly accessible from the internet. You must first connect to the public instance and then connect to the private instance:
```bash
# From your local machine, SSH to the public instance
ssh -i /path/to/roczyno.pem ec2-user@<public-ip-address>

# From the public instance, SSH to the private instance
ssh -i /path/to/roczyno.pem ec2-user@<private-ip-address>
```

## Clean-up Instructions
To avoid incurring charges, delete the CloudFormation stack when you're done:

### Using AWS Management Console
1. Open the AWS Management Console
2. Navigate to CloudFormation service
3. Select your stack
4. Click "Delete" and confirm

### Using AWS CLI
```bash
aws cloudformation delete-stack --stack-name my-vpc-stack
```

## Important Notes
- The NAT Gateway incurs hourly charges and data processing charges
- The template uses the Amazon Linux 2 AMI (ami-06b21ccaeff8cd686)
- The security group allows SSH and HTTP access from any IP (0.0.0.0/0) - consider restricting this for production use
- The key pair name is set to "roczyno" - update this to your own key pair name before deployment

## Customization
You can customize this template by:
- Changing the CIDR blocks for the VPC and subnets
- Modifying the security group rules
- Updating the EC2 instance types
- Adding additional resources as needed

## Troubleshooting
- If stack creation fails, check the CloudFormation events for error messages
- Ensure your AWS account has sufficient permissions to create all resources
- Verify that the specified AMI is available in your region
- Confirm that the key pair exists in your AWS account



## Author
[Jacob Sabbath Adiaba]
