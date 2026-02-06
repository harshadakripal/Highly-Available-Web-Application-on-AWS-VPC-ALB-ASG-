# # Highly Available Web Application on AWS (VPC + ALB + ASG)
Designed and deployed a production‑grade AWS architecture featuring multi‑AZ high availability, secure private compute, load balancing, auto scaling, and NAT‑based outbound access. Includes full documentation, troubleshooting notes, and a professional architecture diagram.

This project demonstrates how to design and deploy a highly available, fault‑tolerant web application** on AWS using a multi‑AZ architecture. It includes a custom VPC, public and private subnets, an Application Load Balancer, EC2 instances in private subnets, and an Auto Scaling Group.

The goal is to simulate a production‑grade environment following AWS best practices for security, scalability, and high availability.

---

## 📌 Architecture Diagram

> The full architecture diagram is available in the `architecture/` folder.

---

## 📌 Architecture Overview

### Core AWS Components
- VPC (10.0.0.0/16)
- Public Subnets (for ALB + NAT Gateway)
- Private Subnets (for EC2 instances)
- Internet Gateway
- NAT Gateway
- Application Load Balancer (ALB)
- Target Group with health checks
- Auto Scaling Group (ASG)
- Security Groups
- Route Tables

### Traffic Flow
- Inbound: Internet → IGW → ALB → Target Group → EC2 (private subnets)
- Outbound: EC2 → NAT Gateway → Internet (package updates)

---

## 📌 What This Project Demonstrates
- Multi‑AZ high availability  
- Secure private compute layer  
- Load balancing and health checks  
- Auto scaling and self‑healing  
- Proper subnetting and routing  
- Infrastructure troubleshooting  
- AWS networking fundamentals  

---

## 📌 Issue Encountered & Fix

### Problem
EC2 instances launched by the Auto Scaling Group appeared in public subnets instead of private ones.

### Root Cause
The ASG was mistakenly configured to use public subnets.

### Fix
- Edited ASG → Network → Selected only:
  - `private-subnet-1`
  - `private-subnet-2`
- Terminated old instances  
- ASG recreated them correctly in private subnets  

### What I Learned
- Subnet names don’t matter — subnet IDs + ASG config determine placement
- A subnet is “public” or “private” based on routing, not naming
- Always validate:
  - Route tables  
  - Subnet associations  
  - ASG subnet selection  

---

## 📌 Repository Structure
aws-ha-webapp/
│
├── architecture/
│   ├── aws-architecture-diagram.png
│
├── docs/
│   ├── project-overview.md
│   ├── troubleshooting.md
│   ├── lessons-learned.md
│   └── cleanup-guide.md
│
├── user-data/
│   └── apache-install.sh
│
└── README.md

