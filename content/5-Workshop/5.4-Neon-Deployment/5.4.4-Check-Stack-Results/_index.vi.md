---
title : "Kiểm tra kết quả Stack và lấy Output quan trọng"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4.4. </b> "
---

Sau khi CloudFormation Stack chuyển sang trạng thái `CREATE_COMPLETE`, hệ thống đã khởi tạo toàn bộ hạ tầng IAM và ngân sách. Bước này hướng dẫn lấy các thông số Output để cấu hình CI/CD và bàn giao tài khoản.

### 5.4.4.1. Bảng giá trị Outputs thu được

Vào tab **Outputs** của Stack `NeonFoodmap-IAM-Setup` để lấy các ARN quan trọng:

| Output Key | Giá trị mẫu | Mục đích sử dụng |
| :--- | :--- | :--- |
| **`GitHubActionsRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-GitHub-Actions-Role` | Lưu vào Secret `AWS_ROLE_ARN` trên GitHub |
| **`ECSBackendRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-ECS-Backend-Role` | Dùng trong ECS Task Definition (Task Role) |
| **`ECSTaskExecutionRoleArn`** | `arn:aws:iam::497172038341:role/NeonFoodmap-ECS-TaskExecution-Role` | Dùng trong ECS Task Definition (Execution Role) |
| **`ConsoleLoginURL`** | `https://497172038341.signin.aws.amazon.com/console` | Đăng nhập Console cho các thành viên trong nhóm |

---

### 5.4.4.2. Tổng quan các tài nguyên IAM đã khởi tạo

#### A. Các Chính sách Tùy chỉnh (Custom Policies)
1. **`NeonFoodmap-Force-MFA`**: Bắt buộc người dùng phải cài đặt xác thực 2 yếu tố (MFA) ngay lần đầu đăng nhập. Nếu chưa bật MFA, mọi thao tác truy cập tài nguyên khác đều bị ngăn chặn.
2. **`NeonFoodmap-Self-Service`**: Cho phép thành viên tự quản lý tài khoản cá nhân (đổi mật khẩu, tạo/xóa Access Key cá nhân).

#### B. Các Nhóm người dùng (User Groups)
1. **`NeonFoodmap-DevOps-Admins`**: Gắn quyền `AdministratorAccess` để quản trị hạ tầng, phân quyền và chi phí.
2. **`NeonFoodmap-Devs`**: Cấp quyền thao tác hạ tầng dịch vụ (ECS, ECR, RDS, S3, CloudWatch, CloudFront).

#### C. Các Vai trò hệ thống (IAM Roles)
1. **`NeonFoodmap-GitHub-Actions-Role`**: Dùng cho pipeline CI/CD từ GitHub qua cơ chế OIDC Federation (không cần lưu static Access Key). Chỉ trust đúng repo `HaoWasabi/NeonFoodmap`.
2. **`NeonFoodmap-ECS-TaskExecution-Role`**: Cho phép ECS agent kéo Docker Image từ ECR và đẩy log lên Amazon CloudWatch.
3. **`NeonFoodmap-ECS-Backend-Role`**: Cấp quyền cho chính ứng dụng Backend (khi đang chạy trên ECS) được phép truy cập S3, RDS Data API và ghi CloudWatch Logs.
4. **`NeonFoodmap-EC2-Backend-Role`**: Tương tự role #3 nhưng dành cho trường hợp chạy Backend trên EC2 thay vì ECS.

#### D. OIDC Identity Provider
Stack tự động khởi tạo Identity Provider với URL `https://token.actions.githubusercontent.com` và Audience `sts.amazonaws.com`, thiết lập liên kết tin cậy giữa GitHub Actions và AWS IAM.
