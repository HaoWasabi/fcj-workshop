---
title : "ECS Services + Auto-Scaling"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1. </b> "
---

### 5.5.1.1. Tạo Backend Service

1. Mở **ECS → Clusters → NeonFoodmap-cluster → Create service**.
2. Chọn task definition family `neonfoodmap-task-be`, đặt service name `svc-neonfoodmap-be` và chọn **Task definition revision latest**.

![picCreateECS12](/images/5-Workshop/5.4-Neon-Deployment/image039.png)

3. Trong phần networking, chọn VPC và các **private subnet** của ứng dụng, chọn ECS task security group.

![image040](/images/5-Workshop/5.4-Neon-Deployment/image040.png)

4. **Load balancing**, chọn **Application Load Balancer** `ALB-NeonFoodMap`; chọn existing listener `80:HTTP`, container backend port `8000` và existing target group `TG-NeonFoodMap-BE`.

![image041](/images/5-Workshop/5.4-Neon-Deployment/image041.png)

5. Kiểm tra cấu hình, chọn **Create**. ECS sẽ tự tạo task, đăng ký IP task vào frontend target group và Cloud Map.

![image042](/images/5-Workshop/5.4-Neon-Deployment/image042.png)

### 5.5.1.2. Tạo Frontend Service

Lặp lại quy trình tạo service cho frontend: chọn task definition `neonfoodmap-task-fe`, service name `svc-neonfoodmap-fe`, private subnet và ECS task security group. **Load balancing**, chọn `ALB-NeonFoodMap`, listener `80:HTTP`, container backend port `8000` và target group `TG-NeonFoodMap-BE`.

Sau khi tạo, ECS thực hiện rolling deployment. Trong thời gian này, giữ task đủ `Healthy` để ALB không chuyển request vào task chưa sẵn sàng.

![image043](/images/5-Workshop/5.4-Neon-Deployment/image043.png)

### 5.5.1.3. Bật Auto Scaling và CPU Policy

1. Mở service `svc-neonfoodmap-be`, vào tab **Service auto scaling** và chọn **Update**.
2. Bật **Use service auto scaling**. Trong phần *Capacity limits*, đặt số task tối thiểu là `2` và tối đa là `6`. Nhờ đó backend luôn có hai task sẵn sàng nhận request, nhưng chỉ được mở rộng tối đa sáu task để kiểm soát chi phí.

![image003](/images/5-Workshop/5.5-Neon-Operations/image003.png)
3. Trong phần scaling policy, chọn **Target tracking**. Đặt tên policy là `cpu-70-target-tracking` và chọn metric **ECSServiceAverageCPUUtilization**. Metric này là mức sử dụng CPU trung bình của tất cả task đang chạy trong service.
4. Đặt *Target value* là `70`. Khi CPU trung bình vượt 70%, ECS Service Auto Scaling sẽ tăng thêm task để chia tải. Khi CPU giảm, ECS có thể giảm task nhưng không thấp hơn giới hạn tối thiểu là hai task.
5. Đặt *Scale-out cooldown* là `60 seconds` và *Scale-in cooldown* là `300 seconds`, sau đó lưu policy. Sau mỗi lần scale-out, ECS chờ 60 giây để task mới khởi động và đăng ký vào target group. Scale-in chờ 5 phút để tránh tình trạng tải dao động làm task bị tăng/giảm liên tục.

![image001](/images/5-Workshop/5.5-Neon-Operations/image001.png)

Sau khi lưu, policy `cpu-70-target-tracking` sẽ theo dõi CPU của service trong giới hạn từ `2` đến `6` task.

![image009](/images/5-Workshop/5.5-Neon-Operations/image009.png)

![image007](/images/5-Workshop/5.5-Neon-Operations/image007.png)
