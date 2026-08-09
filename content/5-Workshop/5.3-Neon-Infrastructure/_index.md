---
title : "Design and Build NeonFoodMap Infrastructure on AWS"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Objectives

In this section, you will deploy the NeonFoodMap infrastructure on AWS through a clear, sequential, and practical end-to-end process. The content is organized step by step, from network setup and resource creation to access configuration and final verification.

### Deployment Architecture Overview

The infrastructure is built using a multi-tier model with the following layers:

- Public subnet: receives traffic from the Internet
- Private subnet: runs applications and internal services
- Database subnet: hosts the system's RDS instance
- S3 bucket: stores frontend assets, media, audio, and logs
- IAM Role and OIDC: grants secure deploy permissions to GitHub Actions

### Deployment Summary

After completing all the steps above, the NeonFoodMap system has a complete foundational infrastructure to operate securely and support continuous deployment:

- VPC and subnets follow the correct multi-tier network model
- NAT Gateway allows private subnets to access the Internet in a controlled manner
- RDS MySQL is deployed in the private subnet, accessible only through the permitted security group
- S3 is configured to store system resources
- IAM Role and GitHub OIDC enable GitHub Actions to deploy to AWS following the principle of least privilege
