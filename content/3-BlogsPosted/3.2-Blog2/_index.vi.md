---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AMAZON RDS AUTOMATION ON SCHEDULE

Xin chào mọi người, mình đang tìm hiểu thêm về cách tối ưu chi phí trên AWS và có đọc được bài hướng dẫn **“Automatically stop and start an Amazon RDS DB instance using AWS Systems Manager Maintenance Windows”** trong AWS Prescriptive Guidance.

Bài viết chia sẻ cách **tự động bật và tắt Amazon RDS** theo lịch, chẳng hạn chỉ khởi động database trong giờ làm việc và tắt vào buổi tối hoặc cuối tuần. Cách này khá phù hợp với các môi trường development, testing hoặc staging không cần hoạt động 24/7.

Ban đầu mình nghĩ muốn làm việc này sẽ phải dùng EventBridge kết hợp với Lambda rồi tự viết code gọi API của RDS. Nhưng AWS Systems Manager đã có sẵn hai Automation runbook là `AWS-StartRdsInstance` và `AWS-StopRdsInstance`, nên gần như không cần viết thêm logic riêng.

Các điểm chính cần nắm:

* Tạo hai Maintenance Windows cho lịch bật và lịch tắt.
* Sử dụng cron expression để xác định thời gian chạy.
* Gắn tag cho các RDS instance cần áp dụng.
* Gom các instance vào Resource Group.
* Dùng Automation runbook để tự động start hoặc stop database.

Điểm mình thấy hay là khi có nhiều RDS instance, mình chỉ cần gắn cùng một tag cho chúng thay vì cấu hình thủ công từng database. Systems Manager sẽ thực hiện task trên toàn bộ tài nguyên trong nhóm.

Một vài điều mình rút ra được:

* Không phải tài nguyên nào cũng cần chạy 24/7, đặc biệt là môi trường dev và test.
* Trước khi viết Lambda để automation, nên kiểm tra xem AWS đã có sẵn runbook phù hợp không.
* IAM role chỉ nên được cấp quyền start và stop đúng những RDS instance cần thiết.
* RDS chỉ có thể bị dừng liên tục tối đa 7 ngày, sau đó AWS sẽ tự khởi động lại để thực hiện bảo trì.

Project của mình hiện vẫn để database chạy liên tục và chỉ tắt khi không sử dụng, nên bài viết này giúp mình biết thêm một cách khá đơn giản để tối ưu chi phí mà không phải xây dựng thêm quá nhiều thành phần.

Cảm ơn mọi người đã đọc bài chia sẻ của mình.

Mình vẫn đang trong quá trình học nên nếu có chỗ nào hiểu chưa đúng, mong mọi người góp ý thêm ạ.

Link bài viết tham khảo: https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-stop-and-start-an-amazon-rds-db-instance-using-aws-systems-manager-maintenance-windows.html

TP. Hồ Chí Minh, 31 tháng 07, 2026 <br>
Diệp Thụy An

![Ảnh minh họa](/images/blog2.png)

[Link bài đăng tại AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2230171591081134/)
