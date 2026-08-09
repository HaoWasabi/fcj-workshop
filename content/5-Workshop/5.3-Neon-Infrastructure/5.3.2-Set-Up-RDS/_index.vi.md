---
title : "Khởi tạo và cấu hình RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---


### 5.3.2.1. Tổng quan kiến trúc triển khai

Hạ tầng cần được xây dựng theo mô hình multi-tier với các lớp sau:

- Public subnet: tiếp nhận lưu lượng từ Internet
- Private subnet: chạy ứng dụng và dịch vụ nội bộ
- Database subnet: triển khai RDS của hệ thống
- S3 bucket: lưu trữ frontend, media, audio, logs
- IAM Role và OIDC: cấp quyền deploy an toàn cho GitHub Actions

### 5.3.2.2. Tạo DB Subnet Group cho Amazon RDS

1. Mở Amazon RDS Console.
2. Chọn Subnet groups.
3. Nhấn Create DB Subnet Group.
4. Chọn VPC của dự án.
5. Chọn các private subnet ở hai AZ.
6. Nhấn Create.

![Hình 31.](/images/5-Workshop/5.3-Neon-Infracstructure/image031.png)


### 5.3.2.3. Tạo DB Parameter Group cho MySQL

1. Vào RDS Console → Parameter groups.
2. Chọn Create parameter group.
3. Cấu hình:
   - Group name: `neonfoodmap-mysql80-params`
   - Description: `Parameter group tối ưu utf8mb4 cho NeonFoodmap`
   - Engine type: `MySQL`
   - Parameter group family: `mysql8.0`
   - Type: `DB Parameter Group`
4. Nhấn Create.
5. Chọn parameter group vừa tạo → Edit.
6. Cập nhật:
   - `character_set_server = utf8mb4`
   - `collation_server = utf8mb4_unicode_ci`
7. Nhấn Save changes.

![Hình 33.](/images/5-Workshop/5.3-Neon-Infracstructure/image033.png)
![Hình 35.](/images/5-Workshop/5.3-Neon-Infracstructure/image035.png)
![Hình 37.](/images/5-Workshop/5.3-Neon-Infracstructure/image037.png)


### 5.3.2.4. Tạo Security Group cho RDS

1. Mở EC2 Console → Security Groups.
2. Chọn Create security group.
3. Thiết lập:
   - Name: `neonfoodmap-rds-sg`
   - Description: `Allow inbound traffic from ECS tasks only`
   - VPC: chọn VPC dự án
4. Trong Inbound rules, thêm rule:
   - Type: `MySQL/Aurora`
   - Port: `3306`
   - Source: security group của ECS task hoặc backend service
5. Giữ nguyên outbound rule mặc định.
6. Nhấn Create security group.

![Hình 39.](/images/5-Workshop/5.3-Neon-Infracstructure/image039.png)

### 5.3.2.5. Khởi tạo Amazon RDS MySQL instance

1. Vào Amazon RDS Console → Databases → Create database.
2. Chọn Standard Create.
3. Chọn engine `MySQL` và phiên bản phù hợp.
4. Trong phần Settings:
   - DB instance identifier: `neonfoodmap-mysql-db`
   - Master username: `admin`
   - Master password: đặt mật khẩu mạnh và lưu lại nơi an toàn
   - Instance type: `db.t3.micro`
5. Trong phần Storage:
   - Storage type: `gp3`
   - Allocated storage: `20 GiB`
   - Tích chọn storage autoscaling tối đa `100 GB`
6. Trong phần Connectivity:
   - Compute resource: `Don't connect to an EC2 compute resource`
   - VPC: chọn VPC dự án
   - DB subnet group: chọn group đã tạo
   - Public access: `No`
   - VPC security group: chọn `neonfoodmap-rds-sg`
   - Database port: `3306`
7. Trong phần Monitoring:
   - Tích chọn `Error log` và `Slow query log`
8. Trong phần Additional configuration:
   - Initial database name: `buocchancoi_db`
   - DB parameter group: `neonfoodmap-mysql80-params`
   - Enable automated backups với retention `7 days`
   - Enable encryption
9. Nhấn Create database.

![Hình 41.](/images/5-Workshop/5.3-Neon-Infracstructure/image041.png)
![Hình 43.](/images/5-Workshop/5.3-Neon-Infracstructure/image043.png)
![Hình 45.](/images/5-Workshop/5.3-Neon-Infracstructure/image045.png)
![Hình 47.](/images/5-Workshop/5.3-Neon-Infracstructure/image047.png)
![Hình 49.](/images/5-Workshop/5.3-Neon-Infracstructure/image049.png)
![Hình 51.](/images/5-Workshop/5.3-Neon-Infracstructure/image051.png)
![Hình 53.](/images/5-Workshop/5.3-Neon-Infracstructure/image053.png)
![Hình 55.](/images/5-Workshop/5.3-Neon-Infracstructure/image055.png)
![Hình 57.](/images/5-Workshop/5.3-Neon-Infracstructure/image057.png)
![Hình 100.](/images/5-Workshop/5.3-Neon-Infracstructure/image100.png)
![Hình 102.](/images/5-Workshop/5.3-Neon-Infracstructure/image102.png)
![Hình 104.](/images/5-Workshop/5.3-Neon-Infracstructure/image104.png)
![Hình 106.](/images/5-Workshop/5.3-Neon-Infracstructure/image106.png)
![Hình 108.](/images/5-Workshop/5.3-Neon-Infracstructure/image108.png)


### 5.3.2.6. Lấy endpoint và lưu vào file `.env`

1. Mở DB instance vừa tạo.
2. Chọn tab Connectivity & security.
3. Copy giá trị Endpoint.
4. Dán vào file `.env` của dự án để backend kết nối đúng địa chỉ database.
