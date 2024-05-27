###### Lab 20: Launching an EC2 Instance 

## Objective
1. Create a private subnet and launch an EC2 instance in it.
2. Configure security groups.
3. Connect to the EC2 instance using a bastion host.

## Prerequisites
- An AWS account.
- AWS CLI installed and configured on your local machine.

## Steps

### 1. Create a VPC
1. Navigate to the VPC dashboard in the AWS Management Console.
2. Click "Create VPC".
3. Provide the following details:
   - Name tag: `Lab20-VPC`
   - IPv4 CIDR block: `10.0.0.0/16`
4. Click "Create VPC".

### 2. Create Subnets
#### Create a Private Subnet
1. Navigate to the VPC dashboard.
2. Click "Subnets" > "Create subnet".
3. Provide the following details:
   - Name tag: `Lab20-Private-Subnet`
   - VPC: Select `Lab20-VPC`
   - Availability Zone: Choose one
   - IPv4 CIDR block: `10.0.1.0/24`
4. Click "Create subnet".

#### Create a Public Subnet
1. Navigate to the VPC dashboard.
2. Click "Subnets" > "Create subnet".
3. Provide the following details:
   - Name tag: `Lab20-Public-Subnet`
   - VPC: Select `Lab20-VPC`
   - Availability Zone: Choose one
   - IPv4 CIDR block: `10.0.0.0/24`
4. Click "Create subnet".

### 3. Create an Internet Gateway
1. Navigate to the VPC dashboard.
2. Click "Internet Gateways" > "Create internet gateway".
3. Name tag: `Lab20-Internet-Gateway`.
4. Click "Create internet gateway".
5. Attach the Internet Gateway to `Lab20-VPC`:
   - Select the created Internet Gateway.
   - Click "Actions" > "Attach to VPC".
   - Select `Lab20-VPC` and click "Attach internet gateway".

### 4. Update Route Table for Public Subnet
1. Navigate to the VPC dashboard.
2. Click "Route Tables" > Select the route table associated with `Lab20-VPC`.
3. Click "Routes" > "Edit routes".
4. Add the following route:
   - Destination: `0.0.0.0/0`
   - Target: `Lab20-Internet-Gateway`
5. Click "Save routes".
6. Associate the route table with the public subnet:
   - Click "Subnet associations" > "Edit subnet associations".
   - Select `Lab20-Public-Subnet` and click "Save".

### 5. Create Security Groups
#### Security Group for Bastion Host
1. Navigate to the EC2 dashboard.
2. Click "Security Groups" > "Create security group".
3. Provide the following details:
   - Security group name: `Lab20-Bastion-SG`
   - Description: `Security group for Bastion Host`
   - VPC: Select `Lab20-VPC`
4. Add an inbound rule:
   - Type: SSH
   - Protocol: TCP
   - Port range: 22
   - Source: Your IP (or specify a range)
5. Click "Create security group".

#### Security Group for Private EC2 Instance
1. Navigate to the EC2 dashboard.
2. Click "Security Groups" > "Create security group".
3. Provide the following details:
   - Security group name: `Lab20-Private-SG`
   - Description: `Security group for private EC2 instance`
   - VPC: Select `Lab20-VPC`
4. Add an inbound rule:
   - Type: Custom TCP
   - Protocol: TCP
   - Port range: 22
   - Source: `Lab20-Bastion-SG`
5. Click "Create security group".

### 6. Launch EC2 Instances
#### Launch Bastion Host in Public Subnet
1. Navigate to the EC2 dashboard.
2. Click "Launch Instance".
3. Configure the following:
   - Name: `Lab20-Bastion-Host`
   - AMI: Amazon Linux 2
   - Instance type: `t2.micro`
   - Key pair: Select or create a key pair
   - Network settings:
     - VPC: `Lab20-VPC`
     - Subnet: `Lab20-Public-Subnet`
     - Auto-assign Public IP: Enable
     - Security group: Select `Lab20-Bastion-SG`
4. Click "Launch instance".

#### Launch Private EC2 Instance in Private Subnet
1. Navigate to the EC2 dashboard.
2. Click "Launch Instance".
3. Configure the following:
   - Name: `Lab20-Private-Instance`
   - AMI: Amazon Linux 2
   - Instance type: `t2.micro`
   - Key pair: Select or create a key pair
   - Network settings:
     - VPC: `Lab20-VPC`
     - Subnet: `Lab20-Private-Subnet`
     - Auto-assign Public IP: Disable
     - Security group: Select `Lab20-Private-SG`
4. Click "Launch instance".

### 7. Connect to the Private EC2 Instance Using Bastion Host
1. Connect to the Bastion Host:
   - Use your terminal or SSH client to connect to the Bastion Host using its public IP and your key pair.
   ```sh
   ssh -i /path/to/your-key-pair.pem ec2-user@<Bastion-Host-Public-IP>
