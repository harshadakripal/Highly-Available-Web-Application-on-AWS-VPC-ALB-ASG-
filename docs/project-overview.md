# Project Overview: Highly Available Web Application on AWS

This document explains how the entire architecture was built, why each component exists, and how they work together to form a production‑style environment.

---

## 1. VPC and Subnet Design

### What was created
- VPC: `10.0.0.0/16`
- Public Subnets:
  - `10.0.1.0/24` (AZ1)
  - `10.0.2.0/24` (AZ2)
- Private Subnets:
  - `10.0.3.0/24` (AZ1)
  - `10.0.4.0/24` (AZ2)

### Why
- Public subnets host internet‑facing components (ALB, NAT).
- Private subnets host EC2 instances for security.
- Multi‑AZ design ensures high availability.

---

## 2. Internet Gateway & NAT Gateway

### Internet Gateway (IGW)
Allows inbound/outbound internet access for public subnets.

### NAT Gateway
Allows private EC2 instances to:
- Install packages
- Perform OS updates  
…without exposing them to the internet.

---

## 3. Route Tables

### Public Route Table
- `0.0.0.0/0 → IGW`

### Private Route Table
- `0.0.0.0/0 → NAT Gateway`

This ensures:
- ALB has internet access
- EC2 instances remain private

---

## 4. Security Groups

### SG‑ALB
- Inbound: HTTP 80 from anywhere

### SG‑EC2
- Inbound: HTTP 80 from SG‑ALB only

This enforces least‑privilege access.

---

## 5. Launch Template

Includes:
- Amazon Linux 2
- Apache installation via user data
- No public IP
- EC2 security group

---

## 6. Target Group & ALB

### Target Group
- Protocol: HTTP 80
- Health check: `/`

### ALB
- Internet‑facing
- Spans both public subnets
- Forwards traffic to target group

---

## 7. Auto Scaling Group

- Min: 2
- Desired: 2
- Max: 4
- Subnets: private‑subnet‑1 & private‑subnet‑2

Ensures:
- High availability
- Self‑healing
- Multi‑AZ redundancy

---

## 8. Validation

- EC2 instances launched in private subnets
- ALB reachable via DNS
- Health checks passed
- ASG replaced terminated instances automatically

---
