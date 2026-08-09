---
title : "Create IAM Stack via CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3. </b> "
---

### 5.4.3.1. Create IAM Stack via AWS CloudFormation

AWS CloudFormation automates the provisioning of all Identity and Access Management (IAM) infrastructure—including Users, Groups, Custom Policies, Roles, and Budget Alerts—using a standardized Infrastructure-as-Code (IaC) YAML template.

> **Prerequisite**: Log in with an AWS Root account or an IAM user with `AdministratorAccess` permissions.

---

### 5.4.3.2. Step-by-Step Instructions:

#### Step 1. Access CloudFormation Service
1. Log in to the **AWS Management Console**.
2. Search for `CloudFormation` in the top search bar and select the service.
3. Verify that the AWS Region selected in the upper-right corner is **Asia Pacific (Singapore) — `ap-southeast-1`**.

#### Step 2. Provision a New Stack from Template
1. Click the **Create stack** button in the upper-right corner and select **With new resources (standard)**.
2. Under **Prerequisite - Prepare template**, select **Template is ready**.
3. Under **Specify template**, select **Upload a template file**.
4. Click **Choose file** and select `neonfoodmap-iam-setup.yaml` located in the project's root directory.
5. Click **Next** to proceed.

#### Step 3. Specify Stack Parameters
1. **Stack name**: Enter `NeonFoodmap-IAM-Setup`.
2. **ProjectName**: Keep default `NeonFoodmap`.
3. **MonthlyBudget**: Set cost monitoring threshold (Default: `15` USD).
4. **AlertEmail**: Provide notification email address for budget threshold alerts.
5. Click **Next**.

#### Step 4. Options Configuration & IAM Capability Acknowledgement
1. Maintain default options on the **Configure stack options** page.
2. Scroll to the bottom of the **Review** step and check the mandatory capability acknowledgment:
   - `[X] I acknowledge that AWS CloudFormation might create IAM resources.`
3. Click **Submit** to trigger the resource deployment.
