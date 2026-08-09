---
title: "Worklog Tuần 5"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5:

Hoàn thành nhiệm vụ CI-001: GitHub Actions CI/CD pipeline theo kế hoạch của Sprint 2.
- Quy trình làm việc (workflow) được kích hoạt khi đẩy code lên nhánh chính (main)
- Xác thực OIDC hoạt động (không sử dụng khóa cứng trong mã nguồn)
- Docker image được build và đẩy lên ECR
- Các bài kiểm thử (test) được thực thi và đạt yêu cầu
- Yêu cầu phê duyệt đối với nhánh chính (main branch)

### Các công việc cần triển khai trong tuần:

| STT | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Tiếp nhận và phân tích yêu cầu của Sprint 2 <br> - Nghiên cứu Acceptance Criteria của AWS-004 | 18/07/2026 | 19/07/2026 | <https://aws.amazon.com/vi/ecr/> |
| 2 | - Tạo tệp quy trình làm việc GitHub Actions (.github/workflows/deploy.yml) <br> - Cấu hình xác thực OIDC (GitHub → AWS) | 18/07/2026 | 19/07/2026 | <https://docs.github.com/en/actions/> |
| 3 | - Thêm bước build Docker (backend + frontend) </br> - Thêm bước đẩy (push) lên ECR <br> - Thêm bước chạy unit test (Django pytest) <br> - Thêm bước chạy E2E test cho frontend (Playwright)| 19/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Kiểm thử quy trình làm việc trên nhánh tính năng (feature branch) <br> - Thiết lập yêu cầu phê duyệt cho nhánh chính (main branch) <br> - Thêm bước kiểm thử nhanh (smoke test) sau khi triển khai| 19/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Hoàn thiện tài liệu triển khai <br> - Chuẩn bị cho Sprint 3 | 20/07/2026 | 21/07/2026   |

### Kết quả đạt được tuần 5:

- Hoàn thành triển khai và cấu hình Task CI-001 theo yêu cầu của Sprint 2.
- Hiểu rõ quy trình triển khai và cấu hình tài nguyên AWS liên quan đến task.
- Kiểm thử thành công và xác nhận tài nguyên hoạt động theo đúng Acceptance Criteria.
- Hoàn thiện tài liệu hướng dẫn triển khai và ghi nhận các vấn đề phát sinh trong quá trình thực hiện.
- Sẵn sàng chuyển sang các nhiệm vụ tiếp theo của Sprint sau khi hoàn thành CI-001.