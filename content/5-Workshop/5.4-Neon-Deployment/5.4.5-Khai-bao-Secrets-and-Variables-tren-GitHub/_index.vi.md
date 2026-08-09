---
title : "Khai báo Secrets và Variables trên GitHub"
date : 2024-01-01
weight : 5
chapter : false
pre : " <b> 5.4.5. </b> "
---

Để pipeline GitHub Actions có thể tương tác an toàn với AWS và đóng gói ứng dụng mà không làm lộ các thông số nhạy cảm trong mã nguồn, bạn cần cấu hình **Repository Secrets** và **Repository Variables**.

---

### 5.4.5.1. Khai báo Repository Secrets

TRUY CẬP: **Settings** → **Secrets and variables** → **Actions** → Nhấn **New repository secret**.

| Secret Name | Mục đích | Giá trị mẫu / Nguồn |
| :--- | :--- | :--- |
| **`AWS_ROLE_ARN`** | Role ARN cấp quyền OIDC cho pipeline | CloudFormation Output `GitHubActionsRoleArn` |
| **`ECS_SUBNETS`** | Danh sách Private Subnets dạng JSON array | `["subnet-0a1b2c3d4e","subnet-0f9e8d7c6b"]` |
| **`ECS_SECURITY_GROUPS`** | Danh sách Security Groups dạng JSON array | `["sg-0123456789abcdef0"]` |
| **`ECS_BACKEND_SERVICE`** | Tên dịch vụ Backend trên ECS | `neonfoodmap-backend-svc` |
| **`ECS_BACKEND_TASK_DEF`** | Tên Task Definition của Backend | `neonfoodmap-backend-td` |
| **`SECRET_KEY`** | Secret key của Django Production | Chuỗi ngẫu nhiên dài ≥ 50 ký tự |
| **`DEBUG`** | Chế độ debug ứng dụng | `False` |
| **`ALLOWED_HOSTS`** | Danh sách tên miền truy cập | Domain/ALB DNS (vd: `api.neonfoodmap.net,*`) |
| **`DB_HOST`** | Địa chỉ Endpoint của RDS MySQL | RDS Console Endpoint |
| **`DB_NAME`** | Tên cơ sở dữ liệu MySQL | `buocchancoi_db` |
| **`DB_USER`** | Tài khoản quản trị Database | `admin` |
| **`DB_PASSWORD`** | Mật khẩu truy cập Database | Mật khẩu RDS vừa khởi tạo |
| **`DB_PORT`** | Cổng kết nối MySQL | `3306` |
| **`CLOUDINARY_URL`** | Kết nối Cloudinary (nếu sử dụng) | Cloudinary Dashboard URL |
| **`GOOGLE_TTS_API_KEY`**| Khóa API Google Text-to-Speech | Google Cloud Console API Key |
| **`PAYPAL_CLIENT_ID`** | PayPal Client ID | PayPal Developer Dashboard |
| **`PAYPAL_SECRET`** | PayPal Client Secret | PayPal Developer Dashboard |
| **`PAYPAL_BASE`** | Base URL của cổng PayPal | `https://api-m.sandbox.paypal.com` |

---

### 5.4.5.2. Khai báo Repository Variables

Chuyển sang tab **Variables** tại **Settings** → **Secrets and variables** → **Actions** → Nhấn **New repository variable**.

| Variable Name | Giá trị mẫu | Ghi chú |
| :--- | :--- | :--- |
| **`AWS_REGION`** | `ap-southeast-1` | AWS Region Singapore |
| **`ECR_REGISTRY`** | `497172038341.dkr.ecr.ap-southeast-1.amazonaws.com` | Đường dẫn ECR Registry |
| **`ECS_CLUSTER`** | `neonfoodmap-cluster` | Tên cụm ECS Cluster |
| **`APP_URL`** | `https://neonfoodmap.example.com` | URL giao diện Frontend |
| **`VITE_API_URL`** | `https://api.neonfoodmap.example.com` | URL API Backend cho Vite build |
