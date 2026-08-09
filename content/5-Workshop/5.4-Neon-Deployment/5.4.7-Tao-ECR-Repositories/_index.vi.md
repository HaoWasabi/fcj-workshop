---
title : "Tạo ECR Repositories"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.4.7. </b> "
---

Amazon Elastic Container Registry (ECR) là dịch vụ lưu trữ và quản lý Docker Images an toàn. Dự án yêu cầu 2 repository riêng biệt cho Backend và Frontend.

---

### 5.4.7.1. Phương pháp Khởi tạo

#### Cách 1: Sử dụng PowerShell Script (Khuyến nghị)
Trong thư mục dự án đã tích hợp sẵn script tự động kiểm tra idempotency và bật cấu hình bảo mật:

```powershell
cd aws_04_deploy
.\01_create_ecr_repos.ps1
```

![Chạy script khởi tạo ECR Repositories](/images/5-Workshop/5.3-Neon-Infracstructure/image084.png)

Kết quả hiển thị URI của từng repository:

![Kết quả khởi tạo ECR Repositories thành công](/images/5-Workshop/5.3-Neon-Infracstructure/image085.png)

#### Cách 2: Sử dụng AWS CLI hoặc Thao tác Thủ công
Chạy các lệnh sau trong terminal (yêu cầu AWS CLI đã xác thực):

```bash
# Tạo Repository cho Backend
aws ecr create-repository \
  --repository-name neonfoodmap-backend \
  --region ap-southeast-1 \
  --image-scanning-configuration scanOnPush=true \
  --encryption-configuration encryptionType=AES256

# Tạo Repository cho Frontend
aws ecr create-repository \
  --repository-name neonfoodmap-frontend \
  --region ap-southeast-1 \
  --image-scanning-configuration scanOnPush=true \
  --encryption-configuration encryptionType=AES256
```

![Tạo ECR Repository thủ công trên AWS Console](/images/5-Workshop/5.3-Neon-Infracstructure/image086.png)

---

### 5.4.7.2. Tiêu chuẩn Bảo mật ECR

Cả 2 repositories đều được thiết lập các cơ chế bảo mật tiêu chuẩn:

| Tính năng | Cấu hình | Ý nghĩa bảo mật |
| :--- | :--- | :--- |
| **Scan On Push** | `scanOnPush = true` | Tự động quét lỗ hổng bảo mật (CVE) mỗi khi có image mới được push |
| **Encryption** | `encryptionType = AES256` | Mã hóa dữ liệu at-rest theo tiêu chuẩn AES-256 |
| **Resource Tags** | `Project=NeonFoodmap` | Đánh tag định danh tài nguyên dự án |

---

### 5.4.7.3. Kiểm tra Kết quả

Chạy lệnh CLI sau để xác minh 2 repositories đã ở trạng thái sẵn sàng:

```bash
aws ecr describe-repositories \
  --repository-names neonfoodmap-backend neonfoodmap-frontend \
  --region ap-southeast-1
```
