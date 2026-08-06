---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# NeonFoodmap Website
## CI/CD Pipeline Automation on the AWS Platform

### 1. Project Overview

This proposal presents a solution for deploying the NeonFoodMap system on the Amazon Web Services (AWS) platform using a Cloud-Native architecture, meeting requirements for scalability, high availability, security, and automated software release processes. The objective of the solution is to build a reusable deployment infrastructure that supports iterative deployments while standardizing operational procedures following DevOps practices in a Production environment.

NeonFoodMap is a food map web platform that allows users to search, discover, and review dining locations in real-time. The system integrates features such as Point of Interest (POI) search, GPS positioning, route display, location reviews, and text-to-speech description playback to enhance user experience. Given its real-time data processing characteristics and concurrent user demands, the system needs to be deployed on a flexible, scalable, highly available, and easily maintainable infrastructure.

The proposal focuses on building a deployment architecture using Docker and Amazon ECS Fargate, source code management via GitHub, automated Build–Test–Deploy workflows through GitHub Actions and OpenID Connect (OIDC), Docker Image storage in Amazon ECR, Amazon RDS database deployment in Private Subnets, static resource management using Amazon S3, and system monitoring via Amazon CloudWatch. The solution aims to establish a unified, secure, and scalable deployment workflow for subsequent phases of the project.

---

### 2. Problem Statement
### Current Status

Prior to implementing the proposal, the NeonFoodMap website project existed merely as standalone application source code (frontend and backend), lacking standardized deployment processes or cloud infrastructure integration. Specifically:

*   **Lack of automation infrastructure:** Application build and deployment processes were manual, with no automated CI/CD pipeline established for the production environment.
*   **Absence of containerization:** The application had not been standardized into Docker images to ensure consistent operation across different environments.
*   **AWS infrastructure not yet established:** AWS components-such as VPC networking, distributed databases, optimized IAM security policies, and monitoring/logging mechanisms-had not yet been built or configured in a unified manner.

### Objectives

The proposal aims for the following technical objectives:

- Automate Build, Test, and Deploy pipelines.
- Eliminate the use of AWS Access Keys in GitHub via OpenID Connect (OIDC).
- Standardize the application deployment process using a Container model.
- Ensure High Availability for the system.
- Support flexible resource scaling based on load demand.
- Establish centralized monitoring, logging, and alerting mechanisms.
- Standardize the deployment process following the DevOps model and improve reusability.

### Solution

- Design the AWS infrastructure architecture.
- Build the CI/CD pipeline.
- Deploy Backend and Frontend using Amazon ECS Fargate.
- Manage Docker Images.
- Configure the database.
- Manage Static Assets.
- Build Logging and Monitoring systems.
- Complete deployment documentation sprint by sprint.

### Return on Investment (ROI)
System standardization and automation deliver practical value:

- **Cost Efficiency:** The Serverless model (ECS Fargate) and Serverless Storage ensure payment only for actual resources used, minimizing idle infrastructure waste.

- **Time-to-Market:** Automated CI/CD pipelines reduce the time required to release new features from hours/days to just a few minutes.

- **High Availability:** Automatically recovering and load-balanced infrastructure achieves high uptime, minimizing service downtime.

- **Security and Better Control:** AWS security standards combined with proactive monitoring systems help protect customer data and proactively detect potential vulnerabilities.

---

### 3. Solution Architecture

![System Architecture Diagram 1](/images/2-Proposal/diagram1.png)

![System Architecture Diagram 2](/images/2-Proposal/diagram2.png)

### List of AWS Services Used
Below is a table listing the AWS services utilized in the project:

| AWS Service | Service Type | Role & Function in the System |
| --- | --- | --- |
| **AWS IAM** | Identity & Access Management | Manages users, groups, roles, and security policies, with Force MFA strictly enforced for all accounts. |
| **VPC** | Networking | Provides a Virtual Private Cloud with CIDR blocks, public and private subnets, route tables, Internet Gateways, and NAT Gateways. |
| **Amazon RDS** | Relational Database | Powers the relational database (RDS MySQL Multi-AZ) to store and manage application data. |
| **Amazon S3** | Object Storage | Stores files using specialized buckets (frontend, media, audio, logs), supporting versioning, lifecycle policies, and encryption. |
| **Amazon ECR** | Container Registry | A repository for Frontend and Backend Docker Container Images. |
| **Amazon ECS** | Container Orchestration | Manages application clusters running via the Fargate launch type. |
| **Application Load Balancer (ALB)** | Load Balancing | Distributes internet HTTP/HTTPS traffic to target groups and supports redirection configuration and health checks. |
| **Amazon CloudWatch** | Monitoring & Observability | Collects logs (CloudWatch Logs), tracks metrics, and configures dashboards and alarms. |
| **Amazon SNS** | Push Notification Service | Sends alert notifications (e.g., billing alerts for costs) to administrators. |
| **AWS CloudFront** | Content Delivery Network (CDN) | Delivers global content, accelerates frontend access, and caches audio files. |

---

### 4. Timeline & Milestones

| Phase | Duration | Main Tasks |
| :--- | :--- | :--- | 
| **Week 1: Research & Design** | 22/06/2026 - 26/06/2026 | - Explore AWS Foundations (Global Infrastructure, IAM, VPC, EC2, S3).<br><br>- Design system architecture (Application, Database, Storage, Networking) and data flow diagrams. |
| **Week 2: Services Exploration & Detailed Design** | 29/06/2026 - 03/07/2026 | - Explore RDS and database migration procedures.<br><br>- Explore ECS/ECR, CloudWatch, SQS, Athena, QuickSight, API Gateway, and Load Balancer.<br><br>- Finalize deployment architecture diagram. |
| **Week 3: Front-end & Back-end Development** | 06/07/2026 - 10/07/2026 | - Develop Frontend (build UI, integrate APIs, Responsive UI).<br><br>- Develop Backend (Database Schema, RESTful API, Authentication/Authorization).<br><br>- Create IAM User, security policies, and setup Billing Alerts. |
| **Week 4: Foundation & Infrastructure** | 13/07/2026 - 17/07/2026 | - Set up Multi-AZ VPC.<br><br>- Provision RDS MySQL.<br><br>- S3 Buckets + Lifecycle + IAM.<br><br>- Configure IAM (CloudFormation).<br><br>- Set up ECR + Docker. |
| **Week 5: CI/CD Pipeline & Deployment** | 20/07/2026 - 24/07/2026 | - Build CI/CD pipeline with GitHub Actions.<br><br>- Configure ECS cluster + task definitions.<br><br>- Configure ALB + Target Groups + Health Checks.<br><br>- Configure Django on AWS.<br><br>- Configure React on AWS. |
| **Week 6-7: Scaling, Monitoring & Go-Live** | 27/07/2026 - 07/08/2026 | - Configure ECS Services + Auto-Scaling.<br><br>- Set up CloudFront + CDN.<br><br>- Deploy CloudWatch dashboard.<br><br>- Cost Monitoring & Alerts.<br><br>- CloudWatch Logs + Log Insights.<br><br>- End-to-End Testing.<br><br>- Finalize deployment documentation. |

---

### 5. Estimated Budget

The system maximizes the use of the **AWS Free Tier** and **Serverless Pay-As-You-Go** model (paying only for actual resources used), helping optimize operational costs to the lowest level.

| AWS Service | Actual Usage / Phase | Estimated Actual Cost (USD) | Role & Function in the System |
| --- | --- | --- | --- |
| **Amazon RDS** | RDS MySQL Multi-AZ (continuous run, with version scale/migration) | **$10.00 - $15.00** | Powers the relational database to store and manage application data. |
| **Amazon S3 & ECR** | File storage (media, logs) and Docker Container Images | **$2.00 - $5.00** | Specialized file storage and Docker Container Image repository for Frontend/Backend. |
| **Amazon ECS & NAT Gateway** | Running container clusters (Fargate) combined with continuous NAT Gateways | **$10.00 - $20.00** | Manages application server clusters and handles network routing. |
| **Application Load Balancer (ALB)** | Distributing internet HTTP/HTTPS traffic | **$3.00 - $6.00** | Distributes incoming traffic to target groups, supporting health checks. |
| **Amazon CloudWatch & SNS** | Collecting logs (CloudWatch Logs), metrics, dashboards, and alarms | **$1.00 - $3.00** | Monitors system health, tracks metrics, and sends alert notifications via SNS. |
| **AWS CloudFront** | CDN content delivery and caching | **$0.00 - $2.00** | Accelerates frontend access and caches audio files. |
| **TOTAL ACTUAL COST** | **System Operation & Testing** | **~$26.00 - ~$51.00 USD / month** | *Closely aligns with actual cost log milestones incurred during testing and resource configuration (actual recorded approx. **$31.52 on 25/07/2026**).* |

---

### 6. Expected Outcomes

Upon completion of the deployment process, the system is expected to achieve the following results:

- Completion of the Cloud-Native deployment architecture on AWS.
- Fully automated CI/CD pipeline from Build to Deploy.
- Applications deployed using Amazon ECS Fargate.
- Docker Images managed centrally on Amazon ECR.
- Database securely deployed within a Private Subnet.
- System monitored via Logging, Monitoring, and Alerting mechanisms.
- Standardized, scalable, and reusable deployment workflow for similar future projects.