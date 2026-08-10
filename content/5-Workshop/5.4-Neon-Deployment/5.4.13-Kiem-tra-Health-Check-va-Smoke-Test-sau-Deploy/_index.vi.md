---
title : "Kiểm tra health check và smoke test sau deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13. </b> "
---

### 5.4.13.1. Kiểm tra health check và smoke test sau deploy

Sau khi ECS task chạy xong, kiểm tra trạng thái target group.

Các mục cần kiểm tra:

- Target group frontend chuyển sang `Healthy`

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image051.png)

- Target group backend chuyển sang `Healthy`

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image050.png)

- ALB DNS có thể truy cập bằng browser
```
http://alb-neonfoodmap-406336237.ap-southeast-1.elb.amazonaws.com/map
```
- Endpoint `/api/...` trả về response hợp lệ

```
http://alb-neonfoodmap-406336237.ap-southeast-1.elb.amazonaws.com/api/
```

![alt text](/images/5-Workshop/5.4-Neon-Deployment/image052.png)


