---
title : "Tạo IAM Stack bằng CloudFormation"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.4.3. </b> "
---

### 5.4.3.1. Tạo IAM Stack trên AWS CloudFormation

AWS CloudFormation cho phép tự động hóa việc khởi tạo toàn bộ hạ tầng IAM (Users, Groups, Custom Policies, Roles và Budget Alerts) cho dự án thông qua file template YAML chuẩn hóa.

> **Yêu cầu tối thiểu**: Đăng nhập bằng tài khoản Root hoặc tài khoản IAM có quyền quản trị `AdministratorAccess`.

---

### 5.4.3.2. Các bước thực hiện chi tiết:

#### Bước 1. Truy cập dịch vụ CloudFormation
1. Đăng nhập vào **AWS Management Console**.
2. Tại ô tìm kiếm dịch vụ ở góc trên, gõ `CloudFormation` và chọn dịch vụ.
3. Đảm bảo vùng chọn (Region) ở góc trên bên phải là **Asia Pacific (Singapore) — `ap-southeast-1`**.

#### Bước 2. Tạo Stack mới từ Template
1. Nhấn nút **Create stack** (ở góc trên bên phải), chọn **With new resources (standard)**.
2. Tại mục **Prerequisite - Prepare template**, chọn **Template is ready**.
3. Tại mục **Specify template**, chọn **Upload a template file**.
4. Nhấp vào **Choose file** và tải lên tệp `neonfoodmap-iam-setup.yaml` từ thư mục gốc của dự án.
5. Nhấn **Next** để chuyển sang bước tiếp theo.

#### Bước 3. Nhập thông số cấu hình (Parameters)
1. **Stack name**: Nhập `NeonFoodmap-IAM-Setup`.
2. **ProjectName**: Giữ mặc định `NeonFoodmap`.
3. **MonthlyBudget**: Nhập hạn mức ngân sách quản lý chi phí (Mặc định: `15` USD).
4. **AlertEmail**: Nhập địa chỉ email nhận cảnh báo khi vượt ngưỡng chi phí ngân sách.
5. Nhấn **Next**.

#### Bước 4. Định cấu hình tùy chọn & Xác nhận quyền
1. Giữ mặc định các thông số tại trang **Configure stack options**.
2. Cuộn xuống cuối trang Review, tích chọn ô bắt buộc:
   - `[X] I acknowledge that AWS CloudFormation might create IAM resources.`
3. Nhấn **Submit** để bắt đầu quá trình tạo tài nguyên.
