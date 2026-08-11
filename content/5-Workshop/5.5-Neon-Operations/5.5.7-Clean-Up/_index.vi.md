---
title : "Dọn dẹp tài nguyên"
date : 2024-01-01
weight : 7
chapter : false
pre : " <b> 5.5.7. </b> "
---

Sau khi hoàn thành workshop, cần dọn dẹp các tài nguyên AWS đã tạo để **tránh phát sinh chi phí không mong muốn**. Thực hiện theo đúng thứ tự dưới đây để đảm bảo không có tài nguyên phụ thuộc nào bị bỏ sót.

---

### 5.5.7.1. Xóa ECS Services và Task Definitions

**ECS Services:**

1. Truy cập **AWS Console** → **Amazon ECS** → **Clusters**.
2. Chọn cluster `neonfoodmap-cluster`.
3. Trong tab **Services**, chọn từng service (`backend-service`, `frontend-service`).
4. Nhấn **Update** → đặt **Desired tasks = 0** → nhấn **Update**.
5. Sau khi tasks dừng hẳn, chọn service → nhấn **Delete**.

**Task Definitions:**

1. Truy cập **Amazon ECS** → **Task Definitions**.
2. Chọn các task definition không còn sử dụng.
3. Chọn từng revision → nhấn **Deregister** để hủy đăng ký.

> **Lưu ý:** Task Definition không thể xóa hoàn toàn, chỉ có thể deregister. Các revision đã deregister sẽ không phát sinh chi phí.

---

### 5.5.7.2. Xóa Application Load Balancer và Target Groups

**Application Load Balancer:**

1. Truy cập **EC2** → **Load Balancers**.
2. Chọn `neonfoodmap-alb`.
3. Nhấn **Actions** → **Delete load balancer** → xác nhận.

**Target Groups:**

1. Truy cập **EC2** → **Target Groups**.
2. Chọn các target group liên quan (`backend-tg`, `frontend-tg`).
3. Nhấn **Actions** → **Delete** → xác nhận.

---

### 5.5.7.3. Xóa CloudFront Distributions

1. Truy cập **AWS Console** → **CloudFront** → **Distributions**.
2. Chọn distribution cần xóa.
3. Nhấn **Disable** và chờ trạng thái chuyển sang **Disabled** (mất vài phút).
4. Sau khi Disabled, chọn distribution → nhấn **Delete** → xác nhận.

> **Lưu ý:** Distribution phải ở trạng thái **Disabled** trước khi có thể xóa.

---

### 5.5.7.4. Xóa S3 Buckets

1. Truy cập **AWS Console** → **S3**.
2. Chọn bucket cần xóa (ví dụ: `neonfoodmap-frontend`, `neonfoodmap-audio`).
3. Nhấn **Empty** để xóa toàn bộ nội dung bucket trước.
4. Sau khi empty xong, nhấn **Delete** → nhập tên bucket để xác nhận.

> **Lưu ý:** Phải xóa hết nội dung bucket trước khi xóa bucket.

---

### 5.5.7.5. Xóa RDS Instance

1. Truy cập **AWS Console** → **RDS** → **Databases**.
2. Chọn `neonfoodmap-db`.
3. Nhấn **Actions** → **Delete**.
4. Tích bỏ chọn **Create final snapshot** (nếu không cần backup).
5. Nhập `delete me` để xác nhận → nhấn **Delete**.

> **Lưu ý:** Việc tạo final snapshot sẽ phát sinh thêm chi phí S3 lưu trữ. Bỏ chọn nếu không có nhu cầu giữ lại dữ liệu.

---

### 5.5.7.6. Xóa ECR Repositories

1. Truy cập **AWS Console** → **Amazon ECR** → **Repositories**.
2. Chọn repository `neonfoodmap-backend` và `neonfoodmap-frontend`.
3. Nhấn **Delete** → nhập `delete` để xác nhận.

---

### 5.5.7.7. Xóa CloudWatch Alarms, Dashboards và Log Groups

**Alarms:**

1. Truy cập **CloudWatch** → **Alarms** → **All alarms**.
2. Chọn tất cả alarm liên quan đến NeonFoodMap.
3. Nhấn **Actions** → **Delete** → xác nhận.

**Dashboards:**

1. Truy cập **CloudWatch** → **Dashboards**.
2. Chọn dashboard `NeonFoodMap-Dashboard`.
3. Nhấn **Delete dashboard** → xác nhận.

**Log Groups:**

1. Truy cập **CloudWatch** → **Log groups**.
2. Chọn các log group liên quan (ví dụ: `/ecs/neonfoodmap-backend`, `/ecs/neonfoodmap-frontend`).
3. Nhấn **Actions** → **Delete log group(s)** → xác nhận.

---

### 5.5.7.8. Xóa VPC và các tài nguyên mạng

> **Thực hiện bước này sau cùng**, vì các tài nguyên khác phụ thuộc vào VPC.

1. Truy cập **VPC** → **Your VPCs**.
2. Chọn `neonfoodmap-vpc`.
3. Trước khi xóa VPC, xóa các tài nguyên phụ thuộc theo thứ tự:
   - **NAT Gateways** → chờ trạng thái **Deleted**.
   - **Subnets** → xóa tất cả subnet trong VPC.
   - **Route Tables** → xóa các route table không phải main.
   - **Internet Gateway** → Detach khỏi VPC trước, sau đó Delete.
   - **Security Groups** → xóa tất cả (trừ default).
4. Quay lại VPC → nhấn **Actions** → **Delete VPC** → xác nhận.

> **Lưu ý:** NAT Gateway phát sinh chi phí theo giờ. Ưu tiên xóa NAT Gateway trước để tránh phát sinh thêm chi phí trong lúc dọn dẹp các tài nguyên khác.

---

### 5.5.7.9. Xóa IAM Roles và Policies

1. Truy cập **IAM** → **Roles**.
2. Tìm và chọn các role liên quan đến workshop (ví dụ: `ecsTaskExecutionRole`, `github-actions-deploy-role`).
3. Nhấn **Delete** → nhập tên role → xác nhận.
4. Truy cập **IAM** → **Policies**.
5. Chọn các custom policy đã tạo → nhấn **Actions** → **Delete** → xác nhận.

---

### 5.5.7.10. Xóa AWS Budget và Cost Anomaly Detection

**Budgets:**

1. Truy cập **AWS Billing** → **Budgets**.
2. Chọn budget `NeonFoodMap-Monthly-Budget`.
3. Nhấn **Delete** → xác nhận.

**Cost Anomaly Detection:**

1. Truy cập **AWS Billing** → **Cost Anomaly Detection**.
2. Chọn monitor đã tạo cho NeonFoodMap.
3. Nhấn **Delete** → xác nhận.

---

### Tổng kết

Sau khi hoàn thành các bước trên, toàn bộ tài nguyên AWS của workshop NeonFoodMap đã được dọn dẹp. Dưới đây là danh sách kiểm tra nhanh:

| Tài nguyên | Trạng thái |
|------------|------------|
| ECS Services & Task Definitions | ✅ Đã xóa / Deregister |
| Application Load Balancer & Target Groups | ✅ Đã xóa |
| CloudFront Distributions | ✅ Đã xóa |
| S3 Buckets | ✅ Đã xóa |
| RDS Instance | ✅ Đã xóa |
| ECR Repositories | ✅ Đã xóa |
| CloudWatch Alarms, Dashboards, Log Groups | ✅ Đã xóa |
| VPC & tài nguyên mạng | ✅ Đã xóa |
| IAM Roles & Policies | ✅ Đã xóa |
| AWS Budget & Cost Anomaly Detection | ✅ Đã xóa |

> Kiểm tra lại **AWS Cost Explorer** sau 24 giờ để xác nhận không còn tài nguyên nào phát sinh chi phí.
