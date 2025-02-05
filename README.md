# AWS WordPress Hosting Project

This repository contains the reference architecture, deployment scripts, and configurations for hosting a WordPress website on Amazon Web Services (AWS). The project leverages a wide range of AWS services to ensure high availability, scalability, and security.

## Table of Contents
1. [Project Overview](#project-overview)  
2. [Architecture Diagram](#architecture-diagram)  
3. [Key AWS Services and Components](#key-aws-services-and-components)  
4. [Deployment Steps](#deployment-steps)  
5. [Repository Structure](#repository-structure)  
6. [How to Use](#how-to-use)  
7. [Contributing](#contributing)  
8. [License](#license)

---

## Project Overview

The goal of this project is to demonstrate how to deploy and manage a WordPress application on AWS using DevOps best practices. The solution ensures:
- **Scalability** via an Auto Scaling Group and load balancing.
- **High Availability** by distributing resources across multiple Availability Zones.
- **Security** through network isolation with public and private subnets, Security Groups, and NAT Gateways.
- **Reliability** by leveraging AWS-managed services such as RDS for the WordPress database and EFS for shared file storage.

---

## Architecture Diagram

A reference diagram of the overall architecture is included in the repository:  
```
[Project Diagram Here]
```
*(Please refer to the `diagram` folder or file in this repository for the full-resolution version.)*

### High-Level Overview
1. **VPC** with both public and private subnets spread across two Availability Zones.
2. **Internet Gateway** attached to the VPC to allow internet access for public subnets.
3. **Security Groups** to restrict inbound and outbound traffic.
4. **EC2 Instance Connect Endpoint** for secure remote connections to instances in private subnets.
5. **NAT Gateway** in a public subnet to provide outbound internet connectivity for resources in private subnets.
6. **Application Load Balancer** distributing traffic to the WordPress instances.
7. **Auto Scaling Group** for EC2 instances to handle scaling based on demand.
8. **Amazon EFS** for shared file system across WordPress instances.
9. **Amazon RDS** for the WordPress MySQL database.
10. **Certificate Manager** for SSL/TLS certificates to secure application communications.
11. **SNS Notifications** integrated with the Auto Scaling Group for alerts.
12. **Route 53** for domain name registration and DNS record management.

---

## Key AWS Services and Components

1. **Virtual Private Cloud (VPC)**  
   - Segregated into public and private subnets for better security and logical separation.
   - Spans multiple Availability Zones to ensure fault tolerance.

2. **Subnets**  
   - **Public Subnets** contain resources that require internet access (e.g., NAT Gateway, Application Load Balancer).  
   - **Private Subnets** host the EC2 instances running the WordPress application and the RDS database.

3. **Internet Gateway (IGW)**  
   - Attached to the VPC to enable internet connectivity for public subnets.

4. **Security Groups**  
   - Act as virtual firewalls for the EC2 instances and the load balancer, permitting only necessary traffic.

5. **NAT Gateway**  
   - Allows EC2 instances in private subnets to access the internet for updates without exposing them publicly.

6. **EC2 Instances & Auto Scaling Group**  
   - **EC2 Instances** host the WordPress application.  
   - **Auto Scaling Group** monitors the health of these instances and automatically scales up or down based on defined metrics.

7. **Application Load Balancer (ALB)**  
   - Distributes incoming HTTP/HTTPS requests across multiple EC2 instances in different Availability Zones.

8. **Amazon EFS**  
   - Provides a shared file system accessible by all EC2 instances running WordPress.

9. **Amazon RDS (MySQL)**  
   - Managed relational database service hosting the WordPress database.

10. **AWS Certificate Manager (ACM)**  
    - Issues and manages SSL/TLS certificates to secure the WordPress site.

11. **AWS Simple Notification Service (SNS)**  
    - Sends alerts and notifications about events in the Auto Scaling Group.

12. **Amazon Route 53**  
    - Handles domain registration and DNS routing to the ALB.

13. **EC2 Instance Connect Endpoint**  
    - Enables secure SSH connections to EC2 instances in private subnets without requiring a public IP.

---

## Deployment Steps

Below is a high-level summary of the deployment process. Refer to the scripts in this repository for detailed instructions:

1. **Create a VPC**  
   - Define CIDR blocks, subnets (public and private), and attach an Internet Gateway.

2. **Set Up Networking Components**  
   - Create and configure a NAT Gateway in a public subnet for outbound internet access from private subnets.
   - Configure route tables for both public and private subnets.

3. **Configure Security**  
   - Define Security Groups for the load balancer and EC2 instances to allow inbound HTTP/HTTPS and SSH (or EC2 Instance Connect) as needed.

4. **Provision an ALB**  
   - Create an Application Load Balancer in public subnets, configure listener rules, and set up a target group for the EC2 instances.

5. **Set Up Auto Scaling Group**  
   - Launch EC2 instances within private subnets.
   - Configure Auto Scaling Group with desired, minimum, and maximum instance counts.  
   - Integrate CloudWatch or custom metrics to trigger scaling policies.

6. **Deploy WordPress**  
   - Use the provided scripts to install WordPress on the EC2 instances.  
   - Mount EFS for shared storage.  
   - Configure RDS endpoint in the WordPress `wp-config.php` file.

7. **Configure SSL Certificates**  
   - Request or import an SSL certificate via AWS Certificate Manager (ACM).  
   - Attach the certificate to the ALB listeners.

8. **Route 53 Setup**  
   - Register your domain (if not already registered).
   - Create a DNS record to point your domain to the ALB.

9. **Set Up SNS Notifications**  
   - Create an SNS topic for Auto Scaling Group activity.
   - Subscribe an email or other endpoint to this SNS topic.

10. **Verify the Setup**  
   - Access your domain and confirm that WordPress loads correctly over HTTPS.
   - Validate Auto Scaling by simulating load or adjusting the minimum/maximum instance counts.

---

## Repository Structure

```
├── diagram/
│   └── aws-wordpress-architecture.png      # Reference architecture diagram
├── scripts/
│   ├── deploy-wordpress.sh                 # Script for WordPress installation/configuration
│   ├── create-ec2-resources.sh             # Script to launch EC2 instances and set up ASG
├── README.md                               # This file
└── ...
```

---

## How to Use

1. **Clone this repository**  
   ```bash
   git clone https://github.com/logunlaja26/Host-WordPress-Website-on-AWS.git
   cd Host-WordPress-Website-on-AWS
   ```

2. **Run scripts**  
   - Execute the scripts in logical order as outlined in the [Deployment Steps](#deployment-steps).

3. **Monitor & Update**  
   - Use AWS SNS and the AWS console to monitor resource health and scaling events.
   - Update your WordPress instances and dependencies as needed.

---

**Thank you for checking out this AWS WordPress Hosting Project!**  
If you have any questions or need further clarification, please [open an issue](../../issues) in the repository. Contributions, suggestions, and improvements are highly encouraged.
