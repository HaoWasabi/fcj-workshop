---
title: "Blog 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---


# MUA SAVINGS PLANS TRƯỚC KHI RIGHTSIZE CÓ THỂ KHIẾN BẠN TỐI ƯU CHI PHÍ SAI THỨ TỰ

Khi hóa đơn EC2 bắt đầu tăng, phản ứng phổ biến của nhiều đội ngũ là **mua Savings Plans càng sớm càng tốt.**

Cách làm này giúp giảm đơn giá compute, nhưng **chưa chắc đã làm hệ thống thực sự hiệu quả.**

Nếu workload vẫn đang over-provisioned, bạn chỉ đang mua mức giá tốt hơn cho một lượng tài nguyên vốn không cần thiết.

*Ngày 16/07/2026, AWS bổ sung Cost Efficiency widget vào Billing and Cost Management Dashboards. Widget này cho phép theo dõi mức độ tối ưu theo thời gian, từng AWS account hoặc Region, đồng thời liên kết trực tiếp tới Cost Optimization Hub để xử lý các cơ hội tiết kiệm.*

Điểm đáng học hỏi không nằm ở việc AWS có thêm một biểu đồ mới, mà ở cách AWS định nghĩa “tối ưu chi phí”.

## COST EFFICIENCY KHÔNG PHẢI LÀ “THÁNG NÀY TIẾT KIỆM ĐƯỢC BAO NHIÊU”
AWS tính Cost Efficiency theo công thức:
```
Cost Efficiency
= [1 − Potential Savings / Total Optimizable Spend] × 100%
```

Ví dụ, nếu doanh nghiệp có 100.000 USD chi phí thuộc phạm vi có thể tối ưu và Cost Optimization Hub phát hiện 10.000 USD cơ hội tiết kiệm, Cost Efficiency sẽ là 90%.

Chỉ số được cập nhật hằng ngày, dựa trên chi phí của 30 ngày gần nhất và các cơ hội tiết kiệm đang tồn tại. Nó tổng hợp cả:
– Tài nguyên nhàn rỗi
– Rightsizing
– Savings Plans và Reserved Instances
– Lựa chọn loại instance phù hợp
– Các đề xuất tối ưu trên EC2, ECS, EKS, EBS, RDS, Lambda, DynamoDB, OpenSearch và nhiều dịch vụ khác

## 3 BÀI HỌC QUAN TRỌNG KHI TỐI ƯU CHI PHÍ AWS
### 1. RIGHTSIZE TRƯỚC, COMMIT SAU

Thứ tự hợp lý nên là:
Xóa tài nguyên idle
→ Rightsize workload
→ Chuyển sang thế hệ instance mới hoặc Graviton khi phù hợp
→ Sau đó mới mua Savings Plans hoặc Reserved Instances.

Nếu mua commitment trước, bạn có thể khóa cam kết chi tiêu trên một workload đang dư thừa. Khi rightsizing sau đó, phần capacity được giải phóng chưa chắc làm hóa đơn giảm tương ứng vì commitment vẫn còn.

AWS cũng ghi nhận rằng nhóm khách hàng lớn kết hợp rightsizing với Savings Plans cải thiện Cost Efficiency nhanh hơn khoảng bốn lần so với nhóm chủ yếu dựa vào Savings Plans.

### 2. KHÔNG CÓ MEMORY METRICS, RIGHTSIZING EC2 DỄ BỊ MÙ

CloudWatch mặc định thu thập nhiều chỉ số EC2, nhưng không tự động có dữ liệu memory utilization từ bên trong hệ điều hành.

Khi thiếu dữ liệu bộ nhớ, Compute Optimizer có thể không đủ thông tin để xác định instance đang dư RAM hay để đề xuất một cấu hình nhỏ hơn.

Trong phân tích trên hơn 71.000 khách hàng AWS đã opt-in, chỉ 17,7% workload đủ điều kiện bật memory metrics. Việc có dữ liệu này liên quan tới mức tiết kiệm trên mỗi recommendation cao hơn từ 8 đến 30 điểm phần trăm, tùy loại instance.
Vì vậy, trước khi kết luận rằng EC2 “không còn gì để tối ưu”, hãy kiểm tra xem CloudWatch Agent hoặc công cụ observability của bạn đã gửi memory metrics hay chưa.

### 3. ĐỪNG BIẾN COST OPTIMIZATION THÀNH CHIẾN DỊCH MỖI QUÝ

Cost Efficiency cập nhật hằng ngày và giờ có thể được đặt cạnh Cost Explorer, Budgets, Savings Plans coverage cùng Reserved Instance utilization trên một dashboard thống nhất.

Dashboard còn có thể chia sẻ cross-account, xuất CSV/PDF hoặc gửi báo cáo định kỳ qua email.

Một quy trình thực tế có thể là:
- Hằng tuần: kiểm tra account hoặc Region có Cost Efficiency giảm mạnh.
- Hằng tháng: review các recommendation có savings cao, effort thấp và khả năng rollback.
- Hằng quý: điều chỉnh commitment dựa trên workload đã được rightsizing, thay vì dựa đơn thuần vào mức chi tiêu hiện tại.

## KẾT LUẬN

Savings Plans không thay thế cho việc tối ưu kiến trúc.
Rightsizing cũng không thay thế cho commitment discount.

Một chiến lược FinOps tốt cần thực hiện đúng thứ tự:
Loại bỏ lãng phí → tối ưu tài nguyên → chuẩn hóa workload → sau đó mới cam kết chi tiêu.

Cost Efficiency widget mới của AWS giúp biến quá trình này từ một danh sách recommendation rời rạc thành một vòng phản hồi có thể đo lường theo thời gian.

Tài liệu tham khảo: 
<br>[1]: **Cost Efficiency widget trong Billing and Cost Management Dashboards** — AWS, 16/07/2026.<br>
https://aws.amazon.com/about-aws/whats-new/2026/07/monitor-cost-efficiency-using-dashboards
<br>[2]: **The AWS State of Cost Efficiency Report** — AWS Cloud Financial Management, 09/06/2026.<br>
https://aws.amazon.com/blogs/aws-cloud-financial-management/the-aws-state-of-cost-efficiency-report/
<br>[3]: **Understanding your cost efficiency metric** — AWS Cost Management Documentation.<br>
https://docs.aws.amazon.com/cost-management/latest/userguide/coh-cost-efficiency.html

TP. Hồ Chí Minh, 04 tháng 08, 2026 <br>
Bùi Bảo Long

[Link bài đăng tại AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2234324983999128/)