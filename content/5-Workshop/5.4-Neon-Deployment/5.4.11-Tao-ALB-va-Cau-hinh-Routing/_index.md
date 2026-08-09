---
title : "Create ALB and Configure Routing"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.4.11. </b> "
---

### 5.4.11.1. Create Security Group for ALB

1. Open EC2 Console → **Security Groups**.
2. Click **Create security group**.
3. Set up:
   - Name: `alb-sg`
   - Description: `Security Group for Public Application Load Balancer`
   - VPC: select the project VPC
4. Add inbound rules:
   - HTTP `80` from `0.0.0.0/0`
   - HTTPS `443` from `0.0.0.0/0`
5. Keep the default outbound rule.

![Figure 11. Create ALB security group](/images/5-Workshop/5.4-neon-deployment/placeholder-alb-sg.png)

### 5.4.11.2. Create Target Groups for Frontend and Backend

- `TG-NeonFoodMap-FE` for frontend
- `TG-NeonFoodMap-BE` for backend

Key configurations:

- Target type: `IP addresses`
- Protocol/Port: frontend `HTTP:80`, backend `HTTP:8000`
- Health check protocol: `HTTP`
- Health check path: `/` or `/api/health`
- Healthy threshold: `2`
- Unhealthy threshold: `2`
- Interval: `30 seconds`

![](/images/5-Workshop/5.4-Neon-Deployment/image025.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image026.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image027.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image028.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image029.png)

![Figure 12. Create target group](/images/5-Workshop/5.4-neon-deployment/placeholder-target-group.png)

### 5.4.11.3. Create Application Load Balancer

1. Open EC2 Console → **Load Balancers**.
2. Select **Create load balancer** → **Application Load Balancer**.
3. Configure:
   - Name: `ALB-NeonFoodMap`
   - Scheme: `Internet-facing`
   - IP address type: `IPv4`
4. Select the appropriate public subnets within the VPC.
5. Select security group `alb-sg`.
6. Configure the `HTTP:80` listener with a default route to the frontend target group.
7. Create the load balancer.

![Figure 13. Create Application Load Balancer](/images/5-Workshop/5.4-neon-deployment/placeholder-alb.png)

![](/images/5-Workshop/5.4-Neon-Deployment/image030.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image031.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image032.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image033.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image034.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image035.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image036.png)
![](/images/5-Workshop/5.4-Neon-Deployment/image037.png)

### 5.4.11.4. Create Listener Rule for API Path

1. Open ALB → **Listeners and rules**.
2. Select the `HTTP:80` listener.
3. Add a rule:
   - Name: `route-backend-api`
   - Condition: `Path /api/*`
   - Action: Forward to the backend target group
4. Save the rule.

![](/images/5-Workshop/5.4-Neon-Deployment/image038.png)

![Figure 84.](/images/5-Workshop/5.5-Neon-Operations/image084.png)

![Figure 85.](/images/5-Workshop/5.5-Neon-Operations/image085.png)

![Figure 86.](/images/5-Workshop/5.5-Neon-Operations/image086.png)

![Figure 87.](/images/5-Workshop/5.5-Neon-Operations/image087.png)

![Figure 83.](/images/5-Workshop/5.5-Neon-Operations/image083.png)
