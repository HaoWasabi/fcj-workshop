---
title : "Initialize and Configure IAM"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4. </b> "
---

### 5.3.4.1. Create IAM Role for ECS Task Execution

1. Open IAM Console → **Roles** → **Create role**.
2. Select trusted entity: `AWS service → Elastic Container Service → ECS Task`.
3. Name the roles:
   - `NeonFoodmap-TaskExecution-Role`
   - `NeonFoodmap-ECS-Backend-Role`
4. Complete the role creation.
5. Attach an inline policy to the backend role with the following permissions:
   - `s3:ListBucket`
   - `s3:GetObject`
   - `s3:PutObject`

![Figure 110.](/images/5-Workshop/5.3-Neon-Infracstructure/image110.png)
![Figure 112.](/images/5-Workshop/5.3-Neon-Infracstructure/image112.png)
![Figure 114.](/images/5-Workshop/5.3-Neon-Infracstructure/image114.png)
![Figure 115.](/images/5-Workshop/5.3-Neon-Infracstructure/image115.png)
![Figure 117.](/images/5-Workshop/5.3-Neon-Infracstructure/image117.png)
![Figure 119.](/images/5-Workshop/5.3-Neon-Infracstructure/image119.png)
![Figure 121.](/images/5-Workshop/5.3-Neon-Infracstructure/image121.png)
![Figure 123.](/images/5-Workshop/5.3-Neon-Infracstructure/image123.png)
![Figure 125.](/images/5-Workshop/5.3-Neon-Infracstructure/image125.png)
![Figure 127.](/images/5-Workshop/5.3-Neon-Infracstructure/image127.png)
![Figure 129.](/images/5-Workshop/5.3-Neon-Infracstructure/image129.png)
![Figure 131.](/images/5-Workshop/5.3-Neon-Infracstructure/image131.png)

### 5.3.4.2. Create GitHub OIDC Provider and IAM Role for GitHub Actions

1. Go to IAM Console → **Access management** → **Identity providers**.
2. Click **Add provider**.
3. Select **OpenID Connect**.
4. Provider URL: `https://token.actions.githubusercontent.com`
5. Audience: `sts.amazonaws.com`
6. Click **Add provider**.
7. Create a new role with trusted entity set to **Web identity**.
8. Select the GitHub OIDC provider you just created.
9. Attach the appropriate policy so GitHub Actions can deploy to S3/ECR/ECS.
10. Configure the trust policy to allow only the specified repository and branch to Assume the Role.

![Figure 133.](/images/5-Workshop/5.3-Neon-Infracstructure/image133.png)
![Figure 135.](/images/5-Workshop/5.3-Neon-Infracstructure/image135.png)
![Figure 137.](/images/5-Workshop/5.3-Neon-Infracstructure/image137.png)
![Figure 139.](/images/5-Workshop/5.3-Neon-Infracstructure/image139.png)
![Figure 142.](/images/5-Workshop/5.3-Neon-Infracstructure/image142.png)
![Figure 143.](/images/5-Workshop/5.3-Neon-Infracstructure/image143.png)
![Figure 145.](/images/5-Workshop/5.3-Neon-Infracstructure/image145.png)
![Figure 149.](/images/5-Workshop/5.3-Neon-Infracstructure/image149.png)
![Figure 151.](/images/5-Workshop/5.3-Neon-Infracstructure/image151.png)
![Figure 153.](/images/5-Workshop/5.3-Neon-Infracstructure/image153.png)
![Figure 155.](/images/5-Workshop/5.3-Neon-Infracstructure/image155.png)
