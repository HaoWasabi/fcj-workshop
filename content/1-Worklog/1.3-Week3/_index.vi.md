---
title: "Worklog Tuần 3"
date: 2026-07-06
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

- Xác định đề tài và phạm vi triển khai của dự án.
- Hoàn thiện thiết kế kiến trúc tổng thể của hệ thống.
- Chuẩn bị kế hoạch triển khai Sprint 1.

### Các công việc cần triển khai trong tuần:

| STT | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Thảo luận và lựa chọn đề tài dự án <br> - Xác định mục tiêu và phạm vi triển khai | 06/07/2026 | 07/07/2026 | |
| 2 | - Phân tích yêu cầu nghiệp vụ <br> - Xác định các chức năng chính của dự án NeonFoodMap | 07/07/2026 | 08/07/2026 | |
| 3 | - Thiết kế kiến trúc triển khai trên AWS <br> - Lựa chọn các dịch vụ phù hợp như VPC, ECS, ECR, RDS, S3, CloudFront <br> - Nghiên cứu thêm về các dịch vụ được sử dụng trong dự án| 08/07/2026 | 10/07/2026 | <https://cloudjourney.awsstudygroup.com> |
| 4 | - Xây dựng quy trình CI/CD cho dự án <br> - Định nghĩa kế hoạch quản lý source code và Docker Image | 09/07/2026 | 10/07/2026 | |
| 5 | - Hoàn thiện tài liệu thiết kế tổng quan <br> - Rà soát các task của Sprint 1 và chuẩn bị môi trường phát triển | 09/07/2026 | 10/07/2026 | |

### Kết quả đạt được tuần 3:

- Hoàn thiện kiến trúc tổng thể của hệ thống trên nền tảng AWS.
- Lựa chọn được các dịch vụ phù hợp để triển khai giải pháp.
- Nghiên cứu và thực hành cơ bản với các dịch vụ khác được sử dụng trong dự án như VPC, RDS, S3, CloudFront.
- Xây dựng hướng đi ban đầu cho quy trình CI/CD và quản lý Container Image.
- Lập kế hoạch tiến độ nhóm cần hoàn thành công việc:
  - **Sprint 1 – Foundation & Infrastructure:** Lập kế hoạch và xây dựng nền tảng AWS cho dự án, gồm VPC Multi-AZ, public/private subnet, Internet Gateway, NAT Gateway và route table.
  - **Sprint 2 – CI/CD Pipeline & Deployment:** Lên kế hoạch triển khai pipeline GitHub Actions, ECS Fargate, Application Load Balancer, health check và cấu hình ứng dụng Django/React trên AWS.
  - **Sprint 3 – Scaling, Monitoring & Go-Live:** Lên kế hoạch hoàn thiện ECS Auto Scaling, CloudFront, CloudWatch, cảnh báo chi phí, kiểm thử end-to-end và tài liệu triển khai.
  - **Sprint 4 – Testing & Documentation:** Kiểm thử toàn bộ luồng chính của ứng dụng, khắc phục lỗi và hoàn thiện tài liệu triển khai, kiến trúc và báo cáo dự án.