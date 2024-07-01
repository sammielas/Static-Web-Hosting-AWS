# Static Website Hosting on AWS

This project demonstrates how I hosted a static HTML web app called rennieG on AWS using various AWS resources to ensure high availability, fault tolerance, and scalability. I have for you below a detailed description of the infrastructure setup and the deployment process.

![Alt text](rennieG.png 'RennieG App Infrastructure on AWS')

## Infrastructure Overview

### VPC and Subnets

- **VPC**: Created a Virtual Private Cloud (VPC) with both public and private subnets spread across two availability zones (AZs) to ensure high availability.
- **Subnets**: Public subnets are used for resources that need direct access to the internet, while private subnets are used for internal resources such as web servers and database servers.

### Internet Gateway

- An **Internet Gateway** is attached to the VPC to enable communication between instances within the VPC and the internet.

### Security Groups

- **Security Groups** are configured to act as a virtual firewall, controlling inbound and outbound traffic to the instances.

### Availability Zones

- Resources are distributed across two Availability Zones for high availability and fault tolerance.

### Public Subnets

- Resources like the NAT Gateway, Bastion Host, and Application Load Balancer are placed in the public subnets.

### Private Subnets

- **Web Servers** and **Database Servers** are hosted in private subnets to protect them from direct internet access.

### NAT Gateway

- A **NAT Gateway** allows private subnet instances to access the internet for updates and patches without exposing them to the public internet.

### EC2 Instance Connect Endpoint

- Used to connect to instances in both public and private subnets securely.

### EC2 Instances

- **EC2 Instances** are used to host the static website, ensuring high availability and scalability through an Auto Scaling Group.

### Application Load Balancer

- An **Application Load Balancer (ALB)** is used to distribute web traffic across multiple EC2 instances in different AZs.

### Auto Scaling Group

- An **Auto Scaling Group** is configured to dynamically adjust the number of EC2 instances based on traffic load, providing scalability, fault tolerance, and elasticity.

### Route 53

- **Route 53** is used for domain registration and DNS routing, ensuring that web traffic is directed to the correct resources.

### GitHub

- **GitHub** is used to store the web application files.

### Amazon Machine Image (AMI)

- Once the website is installed on an EC2 instance, an **AMI** is created to facilitate easy scaling and deployment of additional instances.

## Deployment Script

The following script is used to deploy the web application on an EC2 instance:

```bash
#!/bin/bash
# Switch to the root user
sudo su

# Update all packages
yum update -y

# Install Apache HTTP Server
yum install -y httpd

# Change directory to Apache web root
cd /var/www/html

# Install Git
yum install git -y

# Clone the project repository
git clone https://github.com/sammielas/rennieG.git

# Copy repository contents to web root
cp -R rennieG/. /var/www/html/

# Remove the cloned repository directory
rm -rf rennieG

# Enable Apache HTTP Server on boot
systemctl enable httpd

# Start Apache HTTP Server
systemctl start httpd
```

## Steps to Deploy

1. **Set up VPC and Subnets**: Create a VPC with public and private subnets in two AZs.
2. **Configure Internet Gateway**: Attach an Internet Gateway to the VPC.
3. **Set up Security Groups**: Configure security groups to control traffic.
4. **Deploy NAT Gateway**: Place the NAT Gateway in a public subnet.
5. **Launch EC2 Instances**: Deploy EC2 instances in the private subnets.
6. **Configure Load Balancer**: Set up an Application Load Balancer in the public subnets.
7. **Set up Auto Scaling Group**: Configure the Auto Scaling Group to manage EC2 instances.
8. **Register Domain with Route 53**: Use Route 53 to register the domain and configure DNS settings.
9. **Deploy Web Application**: Use the provided script to install the web application on the EC2 instances.
10. **Create AMI**: After deployment, create an AMI of the configured instance for future scaling.

This setup ensures a highly available, fault-tolerant, and scalable static website hosted on AWS. For further details, you can refer to the project repository on [GitHub](https://github.com/sammielas/rennieG)
