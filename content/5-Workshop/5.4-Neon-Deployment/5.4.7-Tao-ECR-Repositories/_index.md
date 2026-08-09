---
title : "Create ECR Repositories"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7. </b> "
---

Amazon Elastic Container Registry (ECR) is a managed Docker container registry. The architecture requires two distinct repositories to store images for the Backend and Frontend services.

---

### 5.4.7.1. Repository Provisioning Methods

#### Method 1: Execution via PowerShell Script (Recommended)
Run the automated script included in the project directory, which enforces idempotency and security standards:

```powershell
cd aws_04_deploy
.\01_create_ecr_repos.ps1
```

![Executing ECR Repositories creation script](/images/5-Workshop/5.3-Neon-Infracstructure/image084.png)

Expected output displaying URI for each repository:

![ECR Repositories creation result](/images/5-Workshop/5.3-Neon-Infracstructure/image085.png)

#### Method 2: Execution via AWS CLI or Management Console
Execute the following CLI commands (requires configured AWS credentials):

```bash
# Provision Backend Repository
aws ecr create-repository \
  --repository-name neonfoodmap-backend \
  --region ap-southeast-1 \
  --image-scanning-configuration scanOnPush=true \
  --encryption-configuration encryptionType=AES256

# Provision Frontend Repository
aws ecr create-repository \
  --repository-name neonfoodmap-frontend \
  --region ap-southeast-1 \
  --image-scanning-configuration scanOnPush=true \
  --encryption-configuration encryptionType=AES256
```

![Manual ECR Repository creation via AWS Console](/images/5-Workshop/5.3-Neon-Infracstructure/image086.png)

---

### 5.4.7.2. ECR Security Compliance Standards

Both container repositories are configured with enterprise security controls:

| Feature | Configuration | Security Benefit |
| :--- | :--- | :--- |
| **Scan On Push** | `scanOnPush = true` | Automatically scans container images for CVE vulnerabilities upon push |
| **Encryption** | `encryptionType = AES256` | Enforces server-side data encryption at rest using AES-256 |
| **Resource Tags** | `Project=NeonFoodmap` | Establishes resource identification tags for cost tracking |

---

### 5.4.7.3. Verification

Execute the CLI command below to verify repository creation:

```bash
aws ecr describe-repositories \
  --repository-names neonfoodmap-backend neonfoodmap-frontend \
  --region ap-southeast-1
```
