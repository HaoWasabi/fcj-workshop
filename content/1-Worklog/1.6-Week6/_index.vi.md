---
title: "Worklog Tuần 6"
date: 2026-07-27
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

Hoàn thành nhiệm vụ AWS-008: Thiết lập CloudFront + CDN theo kế hoạch của Sprint 3.
- Tạo các tên miền CDN
- Frontend được phân phối qua CloudFront
- Các tệp âm thanh được lưu vào bộ nhớ đệm trên toàn cầu
- HTTPS hoạt động bình thường
- Cache hit rate > 80%

Hoàn thành nhiệm vụ QA-002: End-to-End Testing theo kế hoạch của Sprint 4.
- Tất cả các luồng người dùng quan trọng đều hoạt động tốt
- Không có liên kết hỏng hoặc lỗi 404
- Phát âm thanh qua CloudFront hoạt động bình thường
- Thông báo lỗi hữu dụng
- Hiệu năng ở mức chấp nhận được

### Các công việc cần triển khai trong tuần:

| STT | Công việc | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 1 | - Tiếp nhận và phân tích yêu cầu của Sprint 3 <br> - Nghiên cứu Acceptance Criteria của AWS-008 | 29/07/2026 | 30/07/2026 | <https://aws.amazon.com/vi/cloudfront/> |
| 2 | - Tạo CloudFront distributions | 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Cấu hình hành vi bộ nhớ đệm (cache behaviors) (*.js: 24h, *.html: 1h) <br> - Thiết lập TTL cho bộ nhớ đệm (1 giờ cho frontend, 24 giờ cho audio)| 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Thiết lập HTTPS/SSL cho CloudFront <br> - Thiết lập Origin Access Identity (OAI) cho S3 <br> - Cấu hình nén dữ liệu (gzip, brotli)| 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Kiểm thử hiệu năng CDN (độ trễ từ Singapore) <br> - Theo dõi tỷ lệ cache hit (cache hit ratio)| 30/07/2026 | 31/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Kiểm thử quy trình đăng ký và đăng nhập của người dùng <br> - Kiểm thử tính năng duyệt các điểm tham quan (POI) và tải thông tin mô tả <br> - Kiểm thử tính năng phát âm thanh qua CloudFront <br> - Kiểm thử quy trình đặt tour <br> - Kiểm thử quy trình đặt tour <br> - Kiểm thử tích hợp thanh toán (môi trường Sandbox) <br> - Kiểm thử các tình huống lỗi (dữ liệu không hợp lệ, hết thời gian chờ) <br> - Kiểm thử khả năng hiển thị trên thiết bị di động (responsive)| 30/07/2026 | 31/07/2026   |
| 7 | - Hoàn thiện tài liệu triển khai <br> - Chuẩn bị cho Sprint 3 | 31/07/2026 | 01/08/2026   |

### Kết quả đạt được tuần 6:

- Hoàn thành triển khai và cấu hình Task AWS-008 theo yêu cầu của Sprint 3.
- Hoàn thành kiểm thử Task QA-002 theo yêu cầu của Sprint 3.
- Hiểu rõ quy trình triển khai và cấu hình tài nguyên AWS liên quan đến task.
- Kiểm thử thành công và xác nhận tài nguyên hoạt động theo đúng Acceptance Criteria.
- Hoàn thiện tài liệu hướng dẫn triển khai và ghi nhận các vấn đề phát sinh trong quá trình thực hiện.
- Sẵn sàng chuyển sang các nhiệm vụ tiếp theo của Sprint sau khi hoàn thành AWS-008, QA-002.