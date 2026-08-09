---
title : "Deploying NeonFoodMap on AWS"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Objectives

In this phase, the NeonFoodMap project will be deployed in an automated and containerized way on AWS infrastructure through a clear, sequential, and verifiable end-to-end process.

### Overview

The implementation process includes the following steps:

1. Prepare source code and CI/CD workflow
2. Create IAM stack using CloudFormation
3. Declare Secrets and Variables on GitHub
4. Create ECR repositories
5. Create ECS cluster, task definitions, and services
6. Create ALB and routing rules
7. Run health checks and smoke tests
8. Clean up resources when finished

### Deployment Summary

After completing the above steps, the system is ready to operate in a production-like flow on AWS:

- Code is tested via CI
- Images are built and pushed to ECR
- ECS service runs on Fargate
- ALB distributes traffic to frontend and backend on the correct routes
- Smoke tests confirm the system can handle basic requests
