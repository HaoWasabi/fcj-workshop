---
title : "Thiết kế và xây dựng hạ tầng NeonFoodMap trên AWS"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Mục tiêu

Trong phần này, bạn sẽ triển khai hạ tầng NeonFoodMap trên AWS theo một luồng end-to-end rõ ràng, tuần tự và dễ thực hiện. Nội dung được tổ chức theo từng bước từ khởi tạo mạng, tạo dữ liệu, đến cấu hình quyền truy cập và kiểm tra kết quả cuối cùng.

### Tổng quan kiến trúc triển khai

Hạ tầng cần được xây dựng theo mô hình multi-tier với các lớp sau:

- Public subnet: tiếp nhận lưu lượng từ Internet
- Private subnet: chạy ứng dụng và dịch vụ nội bộ
- Database subnet: triển khai RDS của hệ thống
- S3 bucket: lưu trữ frontend, media, audio, logs
- IAM Role và OIDC: cấp quyền deploy an toàn cho GitHub Actions

### Kết luận triển khai

Sau khi hoàn tất các bước trên, hệ thống NeonFoodMap đã có đầy đủ hạ tầng nền tảng để vận hành an toàn và triển khai liên tục:

- VPC và subnet hoạt động đúng mô hình mạng phân tầng
- NAT Gateway cho phép private subnet truy cập Internet theo kiểm soát
- RDS MySQL đăng ký trong private subnet, truy cập chỉ qua security group được phép
- S3 được cấu hình để lưu trữ tài nguyên hệ thống
- IAM Role và GitHub OIDC giúp GitHub Actions deploy vào AWS theo nguyên tắc least privilege