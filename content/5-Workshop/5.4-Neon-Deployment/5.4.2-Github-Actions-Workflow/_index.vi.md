---
title : "Cấu hình Workflow GitHub Actions"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

## Lấy và Cấu hình GitHub Actions Workflow

Pipeline CI/CD của dự án được tự động hóa hoàn toàn thông qua GitHub Actions. File cấu hình workflow điều phối các công việc từ kiểm thử mã nguồn, đóng gói container image, đẩy lên AWS ECR đến triển khai tự động lên Amazon ECS.

### 5.4.2.2. Lấy file Workflow
Truy cập kho mã nguồn GitHub của dự án và tải/kiểm tra file workflow:
- **Đường dẫn**: `.github/workflows/deploy.yml`
- **URL tham chiếu**: [GitHub Actions deploy.yml](https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml)

---

### 5.4.2.3. Chi tiết các Jobs trong Pipeline

Pipeline bao gồm 6 giai đoạn (Jobs) chính:

#### Job 1: `backend-test` — Backend Lint & Unit Test
| Mục | Chi tiết |
| :--- | :--- |
| **Trigger** | Mọi lệnh Push/PR có thay đổi mã nguồn thuộc `backend/**` |
| **Runner** | `ubuntu-latest` |
| **Môi trường** | Python 3.12 (có tính năng pip cache) |
| **Linting** | `flake8` — kiểm tra cú pháp (lỗi nghiêm trọng E9, F63, F7, F82 sẽ block pipeline) |
| **Testing** | `python manage.py test --settings=config.settings_test` |
| **Database** | Sử dụng SQLite in-memory (không yêu cầu cơ sở dữ liệu MySQL thật) |


#### Job 2: `frontend-check` — Frontend Lint & Build
| Mục | Chi tiết |
| :--- | :--- |
| **Trigger** | Mọi lệnh Push/PR có thay đổi mã nguồn thuộc `frontend/**` |
| **Runner** | `ubuntu-latest` |
| **Môi trường** | Node.js 22 (có tính năng npm cache) |
| **Linting** | `npm run lint` (ESLint) |
| **Building** | `npm run build` (Vite production build) |


#### Job 3: `e2e-tests` — Playwright E2E Tests
| Mục | Chi tiết |
| :--- | :--- |
| **Phụ thuộc** | Yêu cầu `frontend-check` phải vượt qua (Pass) |
| **Browser** | Chromium (Playwright headless) |
| **Phạm vi** | Chạy các test kịch bản trọng yếu (Critical path) |
| **Artifact** | Báo cáo test được lưu trữ 7 ngày trên GitHub Actions |


#### Job 4: `build-and-push` — Build & Push Docker Images
| Mục | Chi tiết |
| :--- | :--- |
| **Phụ thuộc** | Yêu cầu cả `backend-test` và `e2e-tests` đều phải Pass |
| **Điều kiện** | Chỉ kích hoạt khi push trực tiếp hoặc merge vào nhánh `main` |
| **Xác thực AWS** | Sử dụng OIDC để Assume Role vào `AWS_ROLE_ARN` |
| **Tagging** | Gắn tag `latest` và `sha-<7_ký_tự_commit>` |
| **Caching** | Sử dụng GitHub Actions Cache để tối ưu thời gian build |


#### Job 5: `deploy-backend` — Triển khai lên Amazon ECS
| Mục | Chi tiết |
| :--- | :--- |
| **Phụ thuộc** | Yêu cầu Job `build-and-push` thành công |
| **Environment** | `production` (Hỗ trợ cấu hình phê duyệt/Approval) |
| **Chiến lược** | Rolling Update (ECS tự động chuyển đổi lưu lượng) |
| **Migration** | Thực hiện lệnh `run-task` (Fargate task ngắn hạn) để migrate DB |


#### Job 6: `smoke-tests` — Kiểm tra sau Triển khai
| Mục | Chi tiết |
| :--- | :--- |
| **Phụ thuộc** | Yêu cầu Job `deploy-backend` hoàn tất |
| **Health Check** | Truy vấn các endpoint API: `/api/`, `/api/pois/`, `/api/tours/`, `/api/categories/` |
| **Tiêu chí PASS** | Mã trạng thái HTTP trả về < 500 |

---

### 5.4.2.4. Bảng Tổng hợp Điều kiện Kích hoạt (Triggers)

| Sự kiện (Event) | Nhánh (Branches) | Các Jobs được thực thi |
| :--- | :--- | :--- |
| **Push** | `main` | Tất cả 6 jobs (Test → Build → Deploy → Smoke Tests) |
| **Push** | `develop`, `feature/*` | Chỉ chạy 3 jobs kiểm thử: `backend-test`, `frontend-check`, `e2e-tests` |
| **Pull Request** | Gửi vào `main` | Chỉ chạy 3 jobs kiểm thử: `backend-test`, `frontend-check`, `e2e-tests` |

---

### 5.4.2.5. Cơ chế Bảo mật của Pipeline

| Lớp bảo mật | Mô tả chi tiết |
| :--- | :--- |
| **OIDC Federation** | GitHub Actions xác thực với AWS bằng token tạm thời, không lưu Access Key tĩnh |
| **Least Privilege** | GitHub Actions Role chỉ có quyền ECR push + ECS deploy, không cấp quyền Admin |
| **Environment Protection** | Job deploy yêu cầu approval từ reviewer trước khi chính thức khởi chạy |
| **Docker Non-root** | Container chạy bằng user `appuser`, không sử dụng quyền root |
| **Multi-stage Build** | Image production chỉ chứa runtime binaries, không chứa build tools |
| **Secret Management** | Mọi thông tin nhạy cảm lưu trong GitHub Secrets, không commit vào code |
| **Branch Protection** | Chỉ lệnh push/merge vào nhánh `main` mới kích hoạt triển khai production |
