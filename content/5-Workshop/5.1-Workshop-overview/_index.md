---
title : "Introduction"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

# Cloud & DevOps Challenge: Deploying the system to AWS Cloud and building an automated CI/CD pipeline

To meet requirements for flexible deployment, software release automation, and enhanced system availability, this project involves deploying the application on the Amazon Web Services (AWS) platform and establishing a fully automated CI/CD (Continuous Integration/Continuous Deployment) pipeline. The system's Cloud & DevOps architecture is organized into three main processing flows: the CI/CD pipeline, the user request processing flow, and the monitoring & cost management flow.

## 5.1.1. CI/CD Pipeline Flow
The continuous integration and deployment process is designed to automate the entire workflow, spanning from the moment a developer updates the source code to the deployment of the new application version in the production environment. Specifically, when a developer pushes code to the GitHub repository, GitHub Actions is triggered to launch the pipeline. The pipeline utilizes OIDC (OpenID Connect) authentication to interface with AWS Security Token Service (STS) and obtain temporary credentials—replacing static Access Keys/Secret Keys to enhance security. Upon successful authentication, the pipeline builds the application's Docker image and pushes it to Amazon Elastic Container Registry (ECR). Subsequently, the Amazon ECS Fargate service automatically pulls the latest image from ECR and deploys the new application version without manual intervention. The workflow is described as follows:
Developer → GitHub → GitHub Actions → OIDC Authentication → AWS STS → Amazon ECR → Amazon ECS Fargate. 
## 5.1.2. User Request Flow
The request processing flow is designed based on content delivery and load balancing models to ensure access performance and system fault tolerance.
When a user submits an access request, it is first received by Amazon CloudFront—AWS's Content Delivery Network (CDN). For static user interface resources, CloudFront retrieves them directly from the Amazon S3 Frontend Bucket. For dynamic requests, CloudFront forwards the request to the Application Load Balancer (ALB). The ALB distributes traffic to ECS Fargate tasks running across two different Availability Zones (AZs) to ensure High Availability. Application containers connect to the Amazon RDS Primary instance located in Zone A to perform read/write operations. Simultaneously, data is continuously synchronized to the RDS Standby instance in Zone B via the Multi-AZ mechanism, enabling system failover in the event of an incident.
Additionally, ECS containers access the S3 Media Bucket via a VPC Endpoint, allowing internal data transfer within the AWS network without traversing the public Internet, thereby enhancing security and optimizing data transfer costs. The processing flow is described as follows:
User → CloudFront → (S3 Frontend or ALB) → ECS Fargate (2 AZs) → RDS Primary (Zone A) → RDS Standby (Zone B)

## 5.1.3. Observability & Billing
To ensure stable system operation and effective control of cloud resource usage costs, the project implements automated monitoring and alerting mechanisms based on AWS management services. All system logs—including VPC Flow Logs and Application Logs—are collected and centralized in Amazon CloudWatch to facilitate monitoring, analysis, and troubleshooting. The system is configured with Auto Scaling for the ECS service, enabling automatic scaling of the number of tasks when CPU or memory usage exceeds 70%, thereby maintaining performance during periods of high load.
Regarding cost management, the system utilizes AWS Budgets with a monthly budget set at 15 USD. Alert thresholds are configured at 50%, 70%, and 90% of the budget. When costs reach these thresholds or show signs of abnormal increases, AWS Budgets automatically triggers Amazon SNS (Simple Notification Service) to send email alerts to administrators, facilitating early risk detection and timely intervention. The monitoring and alerting workflow is outlined below:
CloudWatch (Logs & Metrics) → Auto Scaling → AWS Budgets → Amazon SNS → Email Alert

## 5.1.4 System Architecture

### Overall Architecture

![](/images/2-Proposal/diagram1.png)

The system architecture is divided into five main layers:

#### CI/CD Layer

- GitHub Repository
- GitHub Actions
- Docker Build
- AWS STS
- Amazon ECR

This layer is responsible for automating the entire deployment process.

After every push to the main branch:

1. Source code is built.
2. A Docker image is created.
3. GitHub uses OIDC for authentication.
4. AWS STS issues temporary credentials.
5. The image is pushed to Amazon ECR.
6. The ECS service performs a rolling deployment.

---

#### Presentation Layer

Includes:

- Amazon CloudFront
- Amazon S3 Static Website

The frontend is hosted on Amazon S3 and distributed via CloudFront to:

- reduce latency
- accelerate access
- reduce backend load

---

#### Application Layer

Includes:

- Application Load Balancer
- Amazon ECS Cluster
- Backend Service
- Frontend Service

The application is deployed on ECS Fargate, eliminating the need to manage EC2 instances.

Independently operating services enable:

- independent component scaling
- rolling updates
- automatic restarts upon failure

---

#### Data Layer

Includes:

- Amazon RDS MySQL
- Multi-AZ Deployment

The database is deployed within a private database subnet.

Using Multi-AZ provides:

- increased fault tolerance
- automatic failover
- reduced service downtime

---

#### Monitoring Layer

Includes:

- Amazon CloudWatch
- Amazon SNS

CloudWatch collects:

- ECS logs
- container logs
- metrics
- application logs

SNS is responsible for sending email alerts when anomalies are detected.

---

### ECS Deployment Architecture

![](/images/2-Proposal/diagram2.png)

The system utilizes an ECS Cluster comprising two services:

#### Backend Service

Deployment:

- Django REST API
- Docker container
- ECS Fargate

The backend handles:

- authentication
- business logic
- database access
- media uploads

---

#### Frontend Service

Deployment:

- React application
- Docker container
- ECS Fargate

The frontend communicates with the backend via the ALB.

---

#### Service Discovery

AWS Cloud Map is used to manage service discovery among containers within the ECS Cluster.

---

#### Load Balancing

The Application Load Balancer receives:

- HTTP requests
- HTTPS requests

It then routes them to the backend service.

---

## 5.1.5. AWS Components Used

| Service | Purpose |
|----------|----------|
| IAM | Access control management |
| STS | Temporary credentials |
| VPC | Private network |
| Public / Private Subnet | Network Segmentation |
| NAT Gateway | Internet access for Private Subnet |
| Internet Gateway | Internet access |
| ECS Fargate | Container execution |
| ECR | Docker image storage |
| RDS MySQL | Database |
| S3 | Static website, media, logs |
| CloudFront | CDN |
| ALB | Load Balancer |
| CloudWatch | Monitoring |
| SNS | Notifications |
| Secrets Manager | Secret management |

---