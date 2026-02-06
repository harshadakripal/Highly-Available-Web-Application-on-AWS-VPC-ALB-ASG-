# Lessons Learned

This project reinforced several key AWS concepts and real‑world troubleshooting skills.

---

## 1. Public vs Private Subnets
A subnet is:
- Public if it routes to an IGW.
- Private if it routes to a NAT Gateway or nowhere.

Names do not matter — routing does.

---

## 2. ASG Subnet Selection Is Critical
Even if subnets are named correctly, the ASG will launch instances wherever you tell it to.

Always double‑check:
- Subnet IDs
- AZ distribution
- Routing

---

## 3. Multi‑AZ Design Improves Reliability
Using two Availability Zones ensures:
- Fault tolerance
- High availability
- Better load distribution

---

## 4. Security Groups Should Be Minimal
- ALB should accept traffic from the internet.
- EC2 should accept traffic only from ALB.
- Outbound rules can remain open.

---

## 5. Infrastructure Debugging Requires Layered Thinking
When something breaks:
1. Check routing  
2. Check subnets  
3. Check security groups  
4. Check ASG  
5. Check user data  
6. Check health checks  

This systematic approach saves time and avoids guesswork.
