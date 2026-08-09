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

NeonFoodMap is a food map website that allows users to search, explore, and review dining locations in real time. The system integrates features such as POI search, GPS positioning, route display, location reviews, and text-to-speech descriptions to improve the user experience. Because the system processes data in real time and must serve many users simultaneously, it requires a flexible, scalable, highly available, and easy-to-maintain infrastructure.

This proposal presents a solution for deploying the NeonFoodMap system on the Amazon Web Services (AWS) platform using a Cloud-Native architecture that meets requirements for scalability, high availability, security, and automated software release processes. The objective is to build a reusable deployment infrastructure that supports iterative deployments and standardizes operational procedures according to DevOps practices in a production environment.

The proposal focuses on building an AWS deployment architecture with Docker and Amazon ECS Fargate, source code management on GitHub, automated Build–Test–Deploy workflows through GitHub Actions and OpenID Connect (OIDC), Docker image storage on Amazon ECR, Amazon RDS deployment in private subnets, static resource management on Amazon S3, and monitoring through Amazon CloudWatch. The solution aims to establish a unified, secure, and scalable deployment workflow for the next phases of the project.

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

#### Overall Architecture
![System Architecture Diagram 1](/images/2-Proposal/diagram1.png)

The deployment architecture is built across two Availability Zones to improve availability:

- **Frontend distribution:** Static content is stored on Amazon S3 Static Website and distributed through Amazon CloudFront to improve access speed for users.
- **API processing:** CloudFront forwards API requests to the ALB. The ALB routes traffic to ECS Fargate tasks in private subnets, and Auto Scaling distributes them across two Availability Zones.
- **Database:** Amazon RDS MySQL is deployed in a Multi-AZ configuration with a primary and a synchronized standby database to improve fault tolerance.
- **CI/CD:** Source code is pushed to GitHub. GitHub Actions uses OIDC to authenticate to AWS STS, builds container images, and pushes them to Amazon ECR; ECS then pulls the images and deploys the new versions.
- **Security and infrastructure:** AWS IAM manages access permissions, AWS Secrets Manager stores sensitive information, and AWS CloudFormation standardizes infrastructure provisioning and changes.
- **System observability:** Amazon CloudWatch collects logs and metrics, while Amazon SNS sends alerts to administrators.

#### Service Connection Architecture
![System Architecture Diagram 2](/images/2-Proposal/diagram2.png)

- Users access the application through the Internet Gateway and Application Load Balancer (ALB).
- The frontend and backend are packaged into containers and run on Amazon ECS Fargate inside the ECS Cluster.
- AWS Cloud Map is used for Service Discovery between containers in the ECS Cluster, allowing services to communicate internally without needing to update IP addresses when task revisions change.

#### Architecture Components

| AWS Service | Service Type | Role & Function in the System |
| --- | --- | --- |
| **AWS IAM** | Identity & Access Management | Manages users, groups, roles, and security policies, with Force MFA strongly recommended for all accounts. |
| **VPC** | Networking | Provides a Virtual Private Cloud with CIDR blocks, public and private subnets, route tables, Internet Gateways, and NAT Gateways. |
| **Amazon RDS** | Relational Database | Supports the relational database (RDS MySQL Multi-AZ) used to store and manage application data. |
| **Amazon S3** | Object Storage | Stores files in dedicated buckets (frontend, media, audio, logs), supporting versioning, lifecycle policies, and encryption. |
| **Amazon ECR** | Container Registry | Stores Docker container images for both the frontend and backend. |
| **Amazon ECS** | Container Orchestration | Manages clusters of applications running with the Fargate launch type. |
| **Application Load Balancer (ALB)** | Load Balancing | Distributes internet HTTP/HTTPS traffic to target groups and supports redirection and health checks. |
| **Amazon CloudWatch** | Monitoring & Observability | Collects logs and metrics, and configures dashboards and alarms. |
| **Amazon SNS** | Notification Service | Sends alert notifications such as billing alerts to administrators. |
| **AWS CloudFront** | Content Delivery Network (CDN) | Delivers content globally, improves frontend access speed, and caches media content. |

---

#### AWS Well-Architected Framework

| Pillar | Applied Solution |
| --- | --- |
| Operational Excellence | GitHub Actions CI/CD, CloudFormation, CloudWatch. |
| Security | IAM Least Privilege, Secrets Manager, KMS, Private Subnets. |
| Reliability | Application Load Balancer, ECS Auto Scaling, RDS Multi-AZ, VPC Endpoint for S3. |
| Performance Efficiency | CloudFront, ECS Fargate Auto Scaling, RDS Optimization. |
| Cost Optimization | ECS Fargate Auto Scaling, S3 Lifecycle. |
| Sustainability | Scale on demand, shut down dev environment outside working hours. |

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

The system makes maximum use of the **AWS Free Tier** and **Serverless Pay-As-You-Go** model, paying only for the resources actually used. This helps optimize operational costs to the lowest possible level.

| AWS Service | Estimated Usage / Phase | Estimated Cost (USD) |
| --- | --- | --- |
| **Amazon ECS (Fargate)** | Running backend containers in production with 2 tasks across 2 AZs and Auto Scaling | **~$10 - $20** |
| **Amazon RDS MySQL** | Multi-AZ production database | **~$10 - $15** |
| **NAT Gateway, ALB, and VPC** | Production networking and load balancing | **~$30 - $40** |
| **Amazon CloudFront** | Static web and API distribution | **~$0 - $2** |
| **Amazon S3** | Static web, media, and logs storage | **~$2 - $5** |
| **Amazon CloudWatch and SNS** | Logs, metrics, alarms, and email alerts | **~$1 - $3** |
| **AWS Secrets Manager and Amazon ECR** | Secret storage and container images | **~$2 - $3** |
| **Estimated total per month** |  | **~$55 - $88** |

In addition, the proposal also applies cost optimization measures such as:
- Configuring **AWS Budgets** and SNS alerts at 50%, 80%, and 100% of the monthly budget.
- Monitoring major cost drivers such as NAT Gateway, ECS Fargate, RDS, and CloudWatch.
- Maintaining only the necessary number of ECS tasks and using Auto Scaling to avoid idle resource allocation.
- Deleting or stopping unused resources in the staging environment after testing.
- Using CloudFront caching for static web and media to reduce origin traffic.

---

### 6. Risk Assessment

#### Risk Matrix

| Risk | Likelihood | Impact |
| --- | --- | --- |
| AWS costs exceed forecast | Medium | Medium |
| ECS task or container failure | Medium | Medium |
| Database incident | Low | High |
| Sensitive information exposure | Low | Very High |
| Sudden traffic spike | Medium | Medium |
| Insufficient logs or alerts | Medium | Medium |
| Error during new version deployment | Medium | Medium |

#### Contingency and Response Plan

- Address cost alerts immediately upon reaching budget thresholds; identify the service generating excess costs and stop or adjust unnecessary resources.
- When API or container errors occur, check CloudWatch Logs, ALB health check status, and ECS task definition before rolling back or deploying a fix.
- In the event of a data incident, prioritize data protection, assess the impact, and execute recovery following tested backup/restore procedures.
- Upon detecting signs of credential exposure, revoke or rotate the secret, review IAM permissions, and audit the deployment history.

---

### 7. Expected Outcomes

After completing the deployment process, the system is expected to achieve the following results:

- **Technical improvement:** Digitizing POI commentary and management, replacing manual information delivery with a multimedia platform that can be monitored, scaled, and automatically deployed on AWS.
- **Long-term value:** Establishing a reusable content and data platform for other tourism areas, while laying the groundwork for expanding user behavior analytics, multilingual content, and collaboration with local businesses in the future.

---

### 8. References

[1]: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

[2]: [The First Cloud Journey](https://cloudjourney.awsstudygroup.com/)

[3]: [AWS Documentation](https://docs.aws.amazon.com/)