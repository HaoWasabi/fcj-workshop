---
title : "Health Checks and Smoke Tests Post-Deploy"
date : 2024-01-01
weight : 13
chapter : false
pre : " <b> 5.4.13. </b> "
---

### 5.4.13.1. Health Checks and Smoke Tests Post-Deploy

After the ECS tasks are running, verify the status of the target groups.

Items to check:

- Frontend target group transitions to `Healthy`

![alt text](image-1.png)

- Backend target group transitions to `Healthy`

![alt text](image.png)

- The ALB DNS is accessible via browser
```
http://alb-neonfoodmap-406336237.ap-southeast-1.elb.amazonaws.com/map
```
- The `/api/...` endpoints return valid responses

```
http://alb-neonfoodmap-406336237.ap-southeast-1.elb.amazonaws.com/api/
```

![alt text](image-2.png)
