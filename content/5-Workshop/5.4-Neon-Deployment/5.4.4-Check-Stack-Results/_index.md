---
title : "Verify Stack Results and Retrieve Outputs"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4. </b> "
---

Once the CloudFormation Stack reaches `CREATE_COMPLETE` status, the IAM infrastructure and budget controls are fully provisioned. This section outlines retrieving Output parameters needed for CI/CD configuration and team onboarding.

### 5.4.4.1. CloudFormation Stack Outputs Summary

Navigate to the **Outputs** tab of stack `NeonFoodmap-IAM-Setup` to extract critical Resource ARNs:

| Output Key | Sample Value | Intended Usage |
| :--- | :--- | :--- |
| **`GitHubActionsRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-GitHub-Actions-Role` | Save to GitHub Secret `AWS_ROLE_ARN` |
| **`ECSBackendRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-ECS-Backend-Role` | Used as Task Role in ECS Task Definition |
| **`ECSTaskExecutionRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-ECS-TaskExecution-Role` | Used as Execution Role in ECS Task Definition |
| **`ConsoleLoginURL`** | `https://497172038341.signin.aws.amazon.com/console` | Console sign-in URL for team members |

---

### 5.4.4.2. Overview of Provisioned IAM Infrastructure

#### A. Custom Security Policies
1. **`NeonFoodmap-Force-MFA`**: Enforces Multi-Factor Authentication (MFA) setup upon initial sign-in. Access to other AWS resources is restricted until MFA is enabled.
2. **`NeonFoodmap-Self-Service`**: Enables team members to self-manage credentials (change passwords, rotate access keys).

#### B. IAM User Groups
1. **`NeonFoodmap-DevOps-Admins`**: Assigned `AdministratorAccess` for infrastructure management and access governance.
2. **`NeonFoodmap-Devs`**: Granted developer permissions for application services (ECS, ECR, RDS, S3, CloudWatch, CloudFront).

#### C. System Roles
1. **`NeonFoodmap-GitHub-Actions-Role`**: Used by GitHub Actions via OIDC Federation to eliminate static Access Keys. Scoped strictly to repository `HaoWasabi/NeonFoodmap`.
2. **`NeonFoodmap-ECS-TaskExecution-Role`**: Grants ECS agents authorization to pull ECR container images and write logs to CloudWatch.
3. **`NeonFoodmap-ECS-Backend-Role`**: Grants the Django backend runtime access to Amazon S3 buckets, RDS Data API, and CloudWatch.
4. **`NeonFoodmap-EC2-Backend-Role`**: Similar to role #3, allocated for EC2-based deployments if EC2 hosting is selected instead of ECS.

#### D. OIDC Identity Provider
Provisioned OpenID Connect Provider pointing to `https://token.actions.githubusercontent.com` with Audience `sts.amazonaws.com` establishing secure trust federations.
