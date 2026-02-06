# Troubleshooting Guide

This document covers issues encountered during the project and how they were resolved.

---

## Issue: EC2 Instances Launching in Public Subnets

### Symptoms
- EC2 instances showed public subnet IDs.
- Instances had public IPs.
- Architecture did not match design.

### Root Cause
The Auto Scaling Group was configured with:
- `public-subnet-1`
- `public-subnet-2`

Instead of:
- `private-subnet-1`
- `private-subnet-2`

### Fix
1. Open ASG → Edit.
2. Update Network section.
3. Select only private subnets.
4. Terminate existing instances.
5. ASG recreated them correctly.

---

## Lessons from this Issue
- Subnet names don’t matter — routing defines public/private.
- ASG subnet selection overrides everything.
- Always validate:
  - Route tables
  - Subnet associations
  - ASG configuration

---

## Other Common Issues

### ALB shows “unhealthy” targets
- Check security groups.
- Ensure Apache is running.
- Verify user data executed correctly.

### ALB DNS not reachable
- Check IGW attachment.
- Check public route table.

### EC2 cannot access internet
- Check NAT Gateway.
- Check private route table.
