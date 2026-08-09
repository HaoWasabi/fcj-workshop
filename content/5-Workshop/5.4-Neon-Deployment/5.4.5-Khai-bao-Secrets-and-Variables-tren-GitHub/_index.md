---
title : "Declare Secrets and Variables on GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5. </b> "
---

To enable GitHub Actions to securely interface with AWS resources and package container images without exposing sensitive parameters in source control, you must configure **Repository Secrets** and **Repository Variables**.

---

### 5.4.5.1. Declare Repository Secrets

NAVIGATE TO: **Settings** → **Secrets and variables** → **Actions** → Click **New repository secret**.

| Secret Name | Purpose | Sample Value / Source |
| :--- | :--- | :--- |
| **`AWS_ROLE_ARN`** | Role ARN for OIDC pipeline authentication | CloudFormation Output `GitHubActionsRoleArn` |
| **`ECS_SUBNETS`** | Target Private Subnet IDs as JSON array | `["subnet-0a1b2c3d4e","subnet-0f9e8d7c6b"]` |
| **`ECS_SECURITY_GROUPS`** | Task Security Group IDs as JSON array | `["sg-0123456789abcdef0"]` |
| **`ECS_BACKEND_SERVICE`** | Backend ECS service identifier | `neonfoodmap-backend-svc` |
| **`ECS_BACKEND_TASK_DEF`** | Backend ECS Task Definition family name | `neonfoodmap-backend-td` |
| **`SECRET_KEY`** | Django production secret key | Random string ≥ 50 characters |
| **`DEBUG`** | Django debug toggle | `False` |
| **`ALLOWED_HOSTS`** | Permitted host headers | Domain/ALB DNS (e.g., `api.neonfoodmap.net,*`) |
| **`DB_HOST`** | RDS MySQL connection endpoint | RDS Console Endpoint |
| **`DB_NAME`** | Database schema name | `buocchancoi_db` |
| **`DB_USER`** | Database administrator username | `admin` |
| **`DB_PASSWORD`** | Database administrator password | Master DB password created earlier |
| **`DB_PORT`** | MySQL connectivity port | `3306` |
| **`CLOUDINARY_URL`** | Cloudinary connection string (optional) | Cloudinary Dashboard URL |
| **`GOOGLE_TTS_API_KEY`**| Google Text-to-Speech API key | Google Cloud Console API Key |
| **`PAYPAL_CLIENT_ID`** | PayPal application client ID | PayPal Developer Dashboard |
| **`PAYPAL_SECRET`** | PayPal application client secret | PayPal Developer Dashboard |
| **`PAYPAL_BASE`** | PayPal API gateway base URL | `https://api-m.sandbox.paypal.com` |

---

### 5.4.5.2. Declare Repository Variables

Switch to the **Variables** tab under **Settings** → **Secrets and variables** → **Actions** → Click **New repository variable**.

| Variable Name | Sample Value | Notes |
| :--- | :--- | :--- |
| **`AWS_REGION`** | `ap-southeast-1` | Target AWS Region (Singapore) |
| **`ECR_REGISTRY`** | `497172038341.dkr.ecr.ap-southeast-1.amazonaws.com` | ECR Registry URI base |
| **`ECS_CLUSTER`** | `neonfoodmap-cluster` | Target ECS Cluster identifier |
| **`APP_URL`** | `https://neonfoodmap.example.com` | Production Frontend web entrypoint |
| **`VITE_API_URL`** | `https://api.neonfoodmap.example.com` | Backend API URL embedded into Vite build |
