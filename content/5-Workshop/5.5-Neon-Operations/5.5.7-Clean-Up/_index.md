---
title : "Clean Up Resources"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7. </b> "
---

After completing the workshop, clean up all AWS resources that were created to **avoid unexpected charges**. Follow the order below to ensure no dependent resources are missed.

---

### 5.5.7.1. Delete ECS Services and Task Definitions

**ECS Services:**

1. Go to **AWS Console** → **Amazon ECS** → **Clusters**.
2. Select the `neonfoodmap-cluster` cluster.
3. In the **Services** tab, select each service (`backend-service`, `frontend-service`).
4. Click **Update** → set **Desired tasks = 0** → click **Update**.
5. Once all tasks have stopped, select the service → click **Delete**.

**Task Definitions:**

1. Go to **Amazon ECS** → **Task Definitions**.
2. Select the task definitions that are no longer in use.
3. Select each revision → click **Deregister**.

> **Note:** Task Definitions cannot be fully deleted — they can only be deregistered. Deregistered revisions do not incur charges.

---

### 5.5.7.2. Delete the Application Load Balancer and Target Groups

**Application Load Balancer:**

1. Go to **EC2** → **Load Balancers**.
2. Select `neonfoodmap-alb`.
3. Click **Actions** → **Delete load balancer** → confirm.

**Target Groups:**

1. Go to **EC2** → **Target Groups**.
2. Select the related target groups (`backend-tg`, `frontend-tg`).
3. Click **Actions** → **Delete** → confirm.

---

### 5.5.7.3. Delete CloudFront Distributions

1. Go to **AWS Console** → **CloudFront** → **Distributions**.
2. Select the distribution to delete.
3. Click **Disable** and wait for the status to change to **Disabled** (this may take a few minutes).
4. Once Disabled, select the distribution → click **Delete** → confirm.

> **Note:** A distribution must be in the **Disabled** state before it can be deleted.

---

### 5.5.7.4. Delete S3 Buckets

1. Go to **AWS Console** → **S3**.
2. Select the buckets to delete (e.g., `neonfoodmap-frontend`, `neonfoodmap-audio`).
3. Click **Empty** to remove all bucket contents first.
4. After emptying, click **Delete** → type the bucket name to confirm.

> **Note:** A bucket must be emptied before it can be deleted.

---

### 5.5.7.5. Delete the RDS Instance

1. Go to **AWS Console** → **RDS** → **Databases**.
2. Select `neonfoodmap-db`.
3. Click **Actions** → **Delete**.
4. Uncheck **Create final snapshot** (if you do not need a backup).
5. Type `delete me` to confirm → click **Delete**.

> **Note:** Creating a final snapshot will incur additional S3 storage costs. Skip this if you do not need to retain the data.

---

### 5.5.7.6. Delete ECR Repositories

1. Go to **AWS Console** → **Amazon ECR** → **Repositories**.
2. Select the `neonfoodmap-backend` and `neonfoodmap-frontend` repositories.
3. Click **Delete** → type `delete` to confirm.

---

### 5.5.7.7. Delete CloudWatch Alarms, Dashboards, and Log Groups

**Alarms:**

1. Go to **CloudWatch** → **Alarms** → **All alarms**.
2. Select all alarms related to NeonFoodMap.
3. Click **Actions** → **Delete** → confirm.

**Dashboards:**

1. Go to **CloudWatch** → **Dashboards**.
2. Select `NeonFoodMap-Dashboard`.
3. Click **Delete dashboard** → confirm.

**Log Groups:**

1. Go to **CloudWatch** → **Log groups**.
2. Select the related log groups (e.g., `/ecs/neonfoodmap-backend`, `/ecs/neonfoodmap-frontend`).
3. Click **Actions** → **Delete log group(s)** → confirm.

---

### 5.5.7.8. Delete the VPC and Network Resources

> **Perform this step last**, as other resources depend on the VPC.

1. Go to **VPC** → **Your VPCs**.
2. Select `neonfoodmap-vpc`.
3. Before deleting the VPC, remove dependent resources in this order:
   - **NAT Gateways** → wait for the status to show **Deleted**.
   - **Subnets** → delete all subnets in the VPC.
   - **Route Tables** → delete all non-main route tables.
   - **Internet Gateway** → Detach from the VPC first, then Delete.
   - **Security Groups** → delete all (except the default group).
4. Return to VPC → click **Actions** → **Delete VPC** → confirm.

> **Note:** NAT Gateways are billed per hour. Prioritize deleting NAT Gateways first to stop charges while cleaning up other resources.

---

### 5.5.7.9. Delete IAM Roles and Policies

1. Go to **IAM** → **Roles**.
2. Find and select the roles created for this workshop (e.g., `ecsTaskExecutionRole`, `github-actions-deploy-role`).
3. Click **Delete** → type the role name → confirm.
4. Go to **IAM** → **Policies**.
5. Select the custom policies that were created → click **Actions** → **Delete** → confirm.

---

### 5.5.7.10. Delete AWS Budget and Cost Anomaly Detection

**Budgets:**

1. Go to **AWS Billing** → **Budgets**.
2. Select `NeonFoodMap-Monthly-Budget`.
3. Click **Delete** → confirm.

**Cost Anomaly Detection:**

1. Go to **AWS Billing** → **Cost Anomaly Detection**.
2. Select the monitor created for NeonFoodMap.
3. Click **Delete** → confirm.

---

### Summary

After completing the steps above, all AWS resources from the NeonFoodMap workshop have been cleaned up. Use the checklist below to verify:

| Resource | Status |
|----------|--------|
| ECS Services & Task Definitions | ✅ Deleted / Deregistered |
| Application Load Balancer & Target Groups | ✅ Deleted |
| CloudFront Distributions | ✅ Deleted |
| S3 Buckets | ✅ Deleted |
| RDS Instance | ✅ Deleted |
| ECR Repositories | ✅ Deleted |
| CloudWatch Alarms, Dashboards, Log Groups | ✅ Deleted |
| VPC & Network Resources | ✅ Deleted |
| IAM Roles & Policies | ✅ Deleted |
| AWS Budget & Cost Anomaly Detection | ✅ Deleted |

> Check **AWS Cost Explorer** after 24 hours to confirm no resources are still generating charges.
