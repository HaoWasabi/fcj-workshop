---
title : "Kiểm tra pipeline CI/CD"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9. </b> "
---

Sau khi hoàn tất cấu hình GitHub Actions, truy cập **GitHub → Actions** để theo dõi quá trình thực thi pipeline. Pipeline được thiết kế gồm **06 job** chạy tuần tự nhằm kiểm tra mã nguồn, xây dựng Docker Image và triển khai ứng dụng lên AWS.

| Thứ tự | Job | Mục đích |
|--------|------------------|-----------------------------------------------|
| 1 | `backend-test` | Kiểm tra chất lượng và Unit Test Backend |
| 2 | `frontend-check` | Kiểm tra Frontend và Build ứng dụng |
| 3 | `e2e-tests` | Chạy Playwright End-to-End Tests |
| 4 | `build-and-push` | Build Docker Image và Push lên Amazon ECR |
| 5 | `deploy-backend` | Triển khai phiên bản mới lên Amazon ECS |
| 6 | `smoke-tests` | Kiểm tra trạng thái dịch vụ sau khi Deploy |

---

### 5.4.9.1. Trigger của Pipeline

| Sự kiện | Branch | Jobs được thực hiện |
|----------|--------|---------------------|
| `push` | `main` | Toàn bộ 6 jobs |
| `push` | `develop`, `feature/**` | `backend-test`, `frontend-check`, `e2e-tests` |
| `pull_request` | `main` | `backend-test`, `frontend-check`, `e2e-tests` |

---

### 5.4.9.2. Chi tiết các Job

#### Job 1 – `backend-test`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Kiểm tra chất lượng mã nguồn Backend |
| Runner | `ubuntu-latest` + Python 3.12 |
| Thực hiện | `flake8` và `python manage.py test` |
| Điều kiện PASS | Không có lỗi lint và toàn bộ Unit Test thành công |

---

#### Job 2 – `frontend-check`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Kiểm tra và Build Frontend |
| Runner | `ubuntu-latest` + Node.js 22 |
| Thực hiện | `npm run lint`, `npm run build` |
| Điều kiện PASS | Build thành công, không có lỗi ESLint |

---

#### Job 3 – `e2e-tests`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Kiểm thử các chức năng quan trọng |
| Công cụ | Playwright (Chromium) |
| Điều kiện | `frontend-check` thành công |
| Kết quả | Báo cáo kiểm thử được lưu trong GitHub Actions |

---

#### Job 4 – `build-and-push`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Build Docker Image và Push lên Amazon ECR |
| Điều kiện | `backend-test` và `e2e-tests` thành công |
| Chỉ chạy | Khi Push vào nhánh `main` |
| Xác thực | GitHub OIDC Assume IAM Role |

---

#### Job 5 – `deploy-backend`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Triển khai phiên bản mới lên Amazon ECS |
| Chiến lược | Rolling Update |
| Migration | Thực hiện bằng ECS Run Task |
| Điều kiện PASS | Service cập nhật thành công |

---

#### Job 6 – `smoke-tests`

| Nội dung | Giá trị |
|----------|----------|
| Mục đích | Kiểm tra dịch vụ sau khi Deploy |
| Endpoint | `/api/`, `/api/pois/`, `/api/tours/`, `/api/categories/` |
| Điều kiện PASS | HTTP Status < 500 |

---

### 5.4.9.3. Xác thực và bảo mật Pipeline

Pipeline sử dụng **GitHub OIDC Federation** để xác thực với AWS thông qua **IAM Role** thay vì lưu Access Key tĩnh trong GitHub Secrets. Điều này giúp tăng tính bảo mật và chỉ cho phép repository được chỉ định thực hiện triển khai.

| Thành phần | Mô tả |
|------------|-------|
| OIDC Federation | Xác thực bằng token tạm thời |
| Least Privilege | IAM Role chỉ cấp quyền cần thiết cho ECR và ECS |
| Environment Protection | Có thể yêu cầu Approval trước khi Deploy Production |
| Secret Management | Thông tin nhạy cảm lưu trong GitHub Secrets |
| Branch Protection | Chỉ nhánh `main` mới thực hiện Deploy |

---

### 5.4.9.4. Xử lý sự cố thường gặp

| Vấn đề | Nguyên nhân | Cách xử lý |
|--------|-------------|------------|
| OIDC login failed | Sai Role ARN hoặc Trust Policy | Kiểm tra `AWS_ROLE_ARN` và cấu hình OIDC |
| Backend test fail | Lỗi mã nguồn hoặc Unit Test | Xem log GitHub Actions và chạy test cục bộ |
| Frontend build fail | Lỗi dependency hoặc TypeScript | Chạy `npm ci` và `npm run build` |
| ECS deploy timeout | Container không khởi động thành công | Kiểm tra CloudWatch Logs và biến môi trường |
| Smoke test fail | Service chưa sẵn sàng hoặc URL sai | Kiểm tra Health Check và Endpoint của ứng dụng |