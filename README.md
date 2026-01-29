# AWS Multi-AZ VPC Deployment with Auto Scaling and Load Balancer

A production-ready, highly available AWS architecture deployed using **VPC**, **public/private subnets**, **Application Load Balancer (ALB)**, **EC2 Auto Scaling**, **Launch Templates**, and **NAT Gateway** to host a secure backend application.

---

## 🚀 Project Overview

This project demonstrates a real-world AWS deployment following best practices for cloud architecture:

- **Custom VPC** with private and public subnets
- **2 Availability Zones** for high availability
- **Public Subnets** hosting ALB and NAT Gateway
- **Private Subnets** hosting EC2 Auto Scaling Group
- **Application Load Balancer (ALB)** distributing incoming traffic
- **NAT Gateway** for outbound internet from private EC2s
- **Launch Templates** with automated application deployment
- **Security Groups, Route Tables, Internet Gateway** configured for secure networking
- Health checks and auto-scaling policies for production readiness

---


## Project Structure
---

AWS-VPC-MULTI-TIER/
├──  architecture diagram/
│   └── architecture-diagram.png
├── projext screenshots/
│ ├── vpc.png
│ ├── subnets.png
│ ├── route-tables.png
│ ├── nat-gateway.png
│ ├── load-balancer.png
│ ├── target-group.png
│ ├── autoscaling-group.png
│ ├── launch-template.png
└── README.md



---

## Components Used

| Component | Purpose |
|-----------|---------|
| **VPC (10.0.0.0/16)** | Custom isolated network |
| **Public Subnets** | ALB & NAT Gateway deployment |
| **Private Subnets** | EC2 Auto Scaling instances |
| **Application Load Balancer (ALB)** | Incoming traffic distribution |
| **Target Group** | EC2 instance health checks & routing |
| **Auto Scaling Group (ASG)** | Dynamic scaling of EC2 instances |
| **Launch Template** | EC2 configuration and user-data script |
| **NAT Gateway** | Outbound internet access for private EC2 |
| **Security Groups** | Controlled ingress/egress traffic |

---

## Deployment Steps

### 1️⃣ Create a Custom VPC
- CIDR block: `10.0.0.0/16`
- Deployed across 2 Availability Zones

### 2️⃣ Create Subnets
- **Public Subnets:** 10.0.1.0/24 (AZ-1), 10.0.2.0/24 (AZ-2)
- **Private Subnets:** 10.0.3.0/24 (AZ-1), 10.0.4.0/24 (AZ-2)

### 3️⃣ Internet Gateway & Route Tables
- Attach Internet Gateway to VPC
- Public subnets → Route Table → 0.0.0.0/0 → IGW
- Private subnets → Route Table → 0.0.0.0/0 → NAT Gateway

### 4️⃣ NAT Gateway
- Deployed in Public Subnet
- Allocated Elastic IP
- Enables private EC2 instances to access the internet

### 5️⃣ Security Groups
- **ALB-SG:** Allow HTTP (80) from Anywhere
- **EC2-SG:** Allow HTTP only from ALB-SG, outbound all traffic

### 6️⃣ Launch Template
- AMI: Amazon Linux 2 / Ubuntu
- Instance Type: t2.micro
- Network: Private Subnets
- Security Group: EC2-SG
- User Data Script (Docker or ECR pull)

### 7️⃣ Auto Scaling Group

Minimum instances: 2

Desired: 2

Maximum: 4

Attached to private subnets and ALB Target Group

Scaling based on CPU utilization

### 8️⃣ Application Load Balancer

Scheme: Internet-facing

Public subnets

Security Group: ALB-SG

Target Group: Private EC2s

Health checks configured 


### 🎯 Key Features

Multi-AZ deployment for high availability

EC2 instances secured in private subnets

Internet access controlled via ALB & NAT Gateway

Automatic scaling based on workload

Production-ready AWS infrastructure