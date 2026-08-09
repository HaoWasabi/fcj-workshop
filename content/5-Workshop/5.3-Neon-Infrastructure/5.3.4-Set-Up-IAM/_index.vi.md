---
title : "Khởi tạo và cấu hình IAM"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.3.4. </b> "
---

### 5.3.4.1. Tạo IAM Role cho ECS task execution

1. Mở IAM Console → Roles → Create role.
2. Chọn trusted entity `AWS service → Elastic Container Service → ECS Task`.
3. Đặt tên role:
   - `NeonFoodmap-TaskExecution-Role`
   - `NeonFoodmap-ECS-Backend-Role`
4. Hoàn tất tạo role.
5. Gắn inline policy cho backend role với quyền:
   - `s3:ListBucket`
   - `s3:GetObject`
   - `s3:PutObject`


![Hình 110.](/images/5-Workshop/5.3-Neon-Infracstructure/image110.png)
![Hình 112.](/images/5-Workshop/5.3-Neon-Infracstructure/image112.png)
![Hình 114.](/images/5-Workshop/5.3-Neon-Infracstructure/image114.png)
![Hình 115.](/images/5-Workshop/5.3-Neon-Infracstructure/image115.png)
![Hình 117.](/images/5-Workshop/5.3-Neon-Infracstructure/image117.png)
![Hình 119.](/images/5-Workshop/5.3-Neon-Infracstructure/image119.png)
![Hình 121.](/images/5-Workshop/5.3-Neon-Infracstructure/image121.png)
![Hình 123.](/images/5-Workshop/5.3-Neon-Infracstructure/image123.png)
![Hình 125.](/images/5-Workshop/5.3-Neon-Infracstructure/image125.png)
![Hình 127.](/images/5-Workshop/5.3-Neon-Infracstructure/image127.png)
![Hình 129.](/images/5-Workshop/5.3-Neon-Infracstructure/image129.png)
![Hình 131.](/images/5-Workshop/5.3-Neon-Infracstructure/image131.png)

### 5.3.4.2. Tạo GitHub OIDC provider và IAM Role cho GitHub Actions

1. Vào IAM Console → Access management → Identity providers.
2. Chọn Add provider.
3. Chọn OpenID Connect.
4. Provider URL: `https://token.actions.githubusercontent.com`
5. Audience: `sts.amazonaws.com`
6. Nhấn Add provider.
7. Tạo role mới với trusted entity là Web identity.
8. Chọn GitHub OIDC provider đã tạo.
9. Gắn policy phù hợp để GitHub Actions triển khai lên S3/ECR/ECS.
10. Cấu hình trust policy để chỉ repository và nhánh được phép Assume Role.

![Hình 133.](/images/5-Workshop/5.3-Neon-Infracstructure/image133.png)
![Hình 135.](/images/5-Workshop/5.3-Neon-Infracstructure/image135.png)
![Hình 137.](/images/5-Workshop/5.3-Neon-Infracstructure/image137.png)
![Hình 139.](/images/5-Workshop/5.3-Neon-Infracstructure/image139.png)
![Hình 142.](/images/5-Workshop/5.3-Neon-Infracstructure/image142.png)
![Hình 143.](/images/5-Workshop/5.3-Neon-Infracstructure/image143.png)
![Hình 145.](/images/5-Workshop/5.3-Neon-Infracstructure/image145.png)
![Hình 149.](/images/5-Workshop/5.3-Neon-Infracstructure/image149.png)
![Hình 151.](/images/5-Workshop/5.3-Neon-Infracstructure/image151.png)
![Hình 153.](/images/5-Workshop/5.3-Neon-Infracstructure/image153.png)
![Hình 155.](/images/5-Workshop/5.3-Neon-Infracstructure/image155.png)