---
title : "Tạo GitHub Environment production"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6. </b> "
---

### 5.4.6.1. Tạo GitHub Environment "production"

GitHub Environment cho phép thiết lập các quy tắc quản trị an toàn (Deployment Protection Rules) như yêu cầu phê duyệt thủ công (Manual Approvals) trước khi mã nguồn được triển khai lên môi trường vĩnh viễn (Production).

---

#### Các bước thực hiện:

1. **Khởi tạo Environment**:
   - Truy cập trang kho lưu trữ GitHub của dự án.
   - Chọn **Settings** → chọn **Environments** ở cột menu bên trái.
   - Nhấn nút **New environment**.

2. **Cấu hình Tên Environment**:
   - Nhập chính xác tên: `production`.
   - Nhấn **Configure environment**.

3. **Cấu hình Quy tắc Phê duyệt (Required Reviewers)**:
   - Tại mục **Deployment protection rules**, tích chọn **Required reviewers**.
   - Tìm kiếm và thêm 1–2 thành viên đại diện team (ví dụ: Lead DevOps/Tech Lead) có quyền phê duyệt deploy.
   - Khi pipeline chạy đến Job `deploy-backend`, GitHub sẽ gửi thông báo phê duyệt đến các reviewer và tạm dừng tới khi nhận được sự chấp thuận.

4. **(Tùy chọn) Thời gian chờ (Wait Timer)**:
   - Nếu muốn tạo khoảng trễ kiểm tra trước khi chính thức chuyển giao traffic, kích hoạt tùy chọn **Wait timer** (ví dụ: `5` phút).
   - Nhấn **Save protection rules** để hoàn tất.
