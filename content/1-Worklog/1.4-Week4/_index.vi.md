---
title: "Worklog Tuần 4"
date: 2026-07-13
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4:

Hoàn thành nhiệm vụ AWS-004: Thiết lập Docker + ECR theo kế hoạch của Sprint 1.
- Các kho lưu trữ ECR đã được tạo và có thể truy cập được
- Các Dockerfile tuân thủ các quy tắc thực hành tốt nhất
- Quá trình build image thành công
- Quá trình pull image từ ECR không gặp lỗi

### Các công việc cần triển khai trong tuần:

| STT | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Tiếp nhận và phân tích yêu cầu của Sprint 1 <br> - Nghiên cứu Acceptance Criteria của AWS-004 | 14/07/2026 | 15/07/2026 | <https://aws.amazon.com/vi/ecr/> |
| 2 | - Tạo các repository ECR (backend, frontend) <br> - Kiểm tra việc đăng nhập ECR từ máy cục bộ | 14/07/2026 | 15/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Build Docker image cho backend (Django + Gunicorn) <br> - Build Docker image cho frontend (React + Nginx) | 14/07/2026 | 15/07/2026 | <https://docs.docker.com/> |
| 4 | - Push các image thử nghiệm lên ECR <br> - Kiểm tra việc pull image từ ECR <br> - Thiết lập chính sách vòng đời (lifecycle policies) cho ECR| 15/07/2026 | 16/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Hoàn thiện tài liệu triển khai <br> - Chuẩn bị cho Sprint 2 | 15/07/2026 | 17/07/2026   |

### Kết quả đạt được tuần 4:

- Hoàn thành triển khai và cấu hình Task AWS-004 theo yêu cầu của Sprint 1.
- Hiểu rõ quy trình triển khai và cấu hình tài nguyên AWS liên quan đến task.
- Kiểm thử thành công và xác nhận tài nguyên hoạt động theo đúng Acceptance Criteria.
- Hoàn thiện tài liệu hướng dẫn triển khai và ghi nhận các vấn đề phát sinh trong quá trình thực hiện.
- Sẵn sàng chuyển sang các nhiệm vụ tiếp theo của Sprint sau khi hoàn thành AWS-004.