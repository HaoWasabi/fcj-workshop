---
title : "Initialize and Configure RDS"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.3.2. </b> "
---

### 5.3.2.1 Deployment Architecture Overview

The infrastructure is built using a multi-tier model with the following layers:

- Public subnet: receives traffic from the Internet
- Private subnet: runs applications and internal services
- Database subnet: hosts the system's RDS instance
- S3 bucket: stores frontend assets, media, audio, and logs
- IAM Role and OIDC: grants secure deploy permissions to GitHub Actions

### 5.3.2.2 Create DB Subnet Group for Amazon RDS

1. Open the Amazon RDS Console.
2. Select **Subnet groups**.
3. Click **Create DB Subnet Group**.
4. Select the project VPC.
5. Select private subnets across two AZs.
6. Click **Create**.

![Figure 31.](/images/5-Workshop/5.3-Neon-Infracstructure/image031.png)

### 5.3.2.3. Create DB Parameter Group for MySQL

1. Go to RDS Console → **Parameter groups**.
2. Click **Create parameter group**.
3. Configure:
   - Group name: `neonfoodmap-mysql80-params`
   - Description: `Parameter group optimized for utf8mb4 for NeonFoodmap`
   - Engine type: `MySQL`
   - Parameter group family: `mysql8.0`
   - Type: `DB Parameter Group`
4. Click **Create**.
5. Select the newly created parameter group → **Edit**.
6. Update the following:
   - `character_set_server = utf8mb4`
   - `collation_server = utf8mb4_unicode_ci`
7. Click **Save changes**.

![Figure 33.](/images/5-Workshop/5.3-Neon-Infracstructure/image033.png)
![Figure 35.](/images/5-Workshop/5.3-Neon-Infracstructure/image035.png)
![Figure 37.](/images/5-Workshop/5.3-Neon-Infracstructure/image037.png)

### 5.3.2.4. Create Security Group for RDS

1. Open EC2 Console → **Security Groups**.
2. Click **Create security group**.
3. Set up:
   - Name: `neonfoodmap-rds-sg`
   - Description: `Allow inbound traffic from ECS tasks only`
   - VPC: select the project VPC
4. Under **Inbound rules**, add a rule:
   - Type: `MySQL/Aurora`
   - Port: `3306`
   - Source: the security group of the ECS task or backend service
5. Keep the default outbound rule unchanged.
6. Click **Create security group**.

![Figure 39.](/images/5-Workshop/5.3-Neon-Infracstructure/image039.png)

### 5.3.2.5. Create Amazon RDS MySQL Instance

1. Go to Amazon RDS Console → **Databases** → **Create database**.
2. Select **Standard Create**.
3. Choose engine `MySQL` and the appropriate version.
4. Under **Settings**:
   - DB instance identifier: `neonfoodmap-mysql-db`
   - Master username: `admin`
   - Master password: set a strong password and store it in a safe place
   - Instance type: `db.t3.micro`
5. Under **Storage**:
   - Storage type: `gp3`
   - Allocated storage: `20 GiB`
   - Enable storage autoscaling up to `100 GB`
6. Under **Connectivity**:
   - Compute resource: `Don't connect to an EC2 compute resource`
   - VPC: select the project VPC
   - DB subnet group: select the group you created
   - Public access: `No`
   - VPC security group: select `neonfoodmap-rds-sg`
   - Database port: `3306`
7. Under **Monitoring**:
   - Enable `Error log` and `Slow query log`
8. Under **Additional configuration**:
   - Initial database name: `buocchancoi_db`
   - DB parameter group: `neonfoodmap-mysql80-params`
   - Enable automated backups with a retention period of `7 days`
   - Enable encryption
9. Click **Create database**.

![Figure 41.](/images/5-Workshop/5.3-Neon-Infracstructure/image041.png)
![Figure 43.](/images/5-Workshop/5.3-Neon-Infracstructure/image043.png)
![Figure 45.](/images/5-Workshop/5.3-Neon-Infracstructure/image045.png)
![Figure 47.](/images/5-Workshop/5.3-Neon-Infracstructure/image047.png)
![Figure 49.](/images/5-Workshop/5.3-Neon-Infracstructure/image049.png)
![Figure 51.](/images/5-Workshop/5.3-Neon-Infracstructure/image051.png)
![Figure 53.](/images/5-Workshop/5.3-Neon-Infracstructure/image053.png)
![Figure 55.](/images/5-Workshop/5.3-Neon-Infracstructure/image055.png)
![Figure 57.](/images/5-Workshop/5.3-Neon-Infracstructure/image057.png)
![Figure 100.](/images/5-Workshop/5.3-Neon-Infracstructure/image100.png)
![Figure 102.](/images/5-Workshop/5.3-Neon-Infracstructure/image102.png)
![Figure 104.](/images/5-Workshop/5.3-Neon-Infracstructure/image104.png)
![Figure 106.](/images/5-Workshop/5.3-Neon-Infracstructure/image106.png)
![Figure 108.](/images/5-Workshop/5.3-Neon-Infracstructure/image108.png)

### 5.3.2.6. Retrieve Endpoint and Save to `.env` File

1. Open the DB instance you just created.
2. Select the **Connectivity & security** tab.
3. Copy the **Endpoint** value.
4. Paste it into the project's `.env` file so the backend can connect to the correct database address.
