---
title : "Triển khai NeonFoodMap trên AWS"
date : 2024-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Mục tiêu

Trong giai đoạn này, dự án NeonFoodMap sẽ được triển khai tự động hóa và container hóa lên hạ tầng AWS theo một luồng end-to-end rõ ràng, tuần tự và có thể kiểm tra được. 

### Tổng quan

Quy trình thực hiện bao gồm các bước như sau:

1. Chuẩn bị mã nguồn và workflow CI/CD
2. Tạo stack IAM bằng CloudFormation
3. Khai báo Secrets và Variables trên GitHub
4. Tạo ECR repositories
5. Tạo ECS cluster, task definition và service
6. Tạo ALB và routing rule
7. Kiểm tra health check và smoke test
8. Dọn dẹp tài nguyên khi kết thúc

### Kết luận triển khai

Sau khi hoàn thành các bước trên, hệ thống đã sẵn sàng hoạt động theo luồng production-like trên AWS:

- Code được kiểm thử bằng CI
- Image được build và push lên ECR
- ECS service chạy bằng Fargate
- ALB phân phối traffic đến frontend và backend đúng route
- Smoke test xác nhận hệ thống có thể đáp ứng request cơ bản