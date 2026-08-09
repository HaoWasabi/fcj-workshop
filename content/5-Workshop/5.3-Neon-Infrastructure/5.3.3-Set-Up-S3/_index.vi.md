---
title : "Khởi tạo và cấu hình S3"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3.3. </b> "
---

### 5.3.3.1. Tạo 4 bucket S3 cho hệ thống

Tạo các bucket theo chức năng sau:

- `frontend`
- `media`
- `audio`
- `logs`

Các bước:

1. Mở Amazon S3 Console.
2. Chọn Create bucket.
3. Nhập tên bucket duy nhất trên AWS.
4. Chọn cùng region với VPC và RDS.
5. Giữ `Block all public access` ở trạng thái Enabled.
6. Nhấn Create bucket.
7. Lặp lại cho đủ 4 bucket.

![Hình 59.](/images/5-Workshop/5.3-Neon-Infracstructure/image059.png)
![Hình 61.](/images/5-Workshop/5.3-Neon-Infracstructure/image061.png)


### 5.3.3.2. Bật Versioning cho media và audio bucket

1. Chọn bucket `neonfoodmap-media-dev`.
2. Vào tab Properties.
3. Chọn Bucket Versioning → Edit.
4. Chọn Enable.
5. Lặp lại tương tự với bucket `neonfoodmap-audio-dev`.

![Hình 63.](/images/5-Workshop/5.3-Neon-Infracstructure/image063.png)
![Hình 65.](/images/5-Workshop/5.3-Neon-Infracstructure/image065.png)
![Hình 67.](/images/5-Workshop/5.3-Neon-Infracstructure/image067.png)
![Hình 69.](/images/5-Workshop/5.3-Neon-Infracstructure/image069.png)
![Hình 71.](/images/5-Workshop/5.3-Neon-Infracstructure/image071.png)
![Hình 75.](/images/5-Workshop/5.3-Neon-Infracstructure/image075.png)
![Hình 77.](/images/5-Workshop/5.3-Neon-Infracstructure/image077.png)
![Hình 78.](/images/5-Workshop/5.3-Neon-Infracstructure/image078.png)
![Hình 80.](/images/5-Workshop/5.3-Neon-Infracstructure/image080.png)
![Hình 82.](/images/5-Workshop/5.3-Neon-Infracstructure/image082.png)
![Hình 83.](/images/5-Workshop/5.3-Neon-Infracstructure/image083.png)
![Hình 85.](/images/5-Workshop/5.3-Neon-Infracstructure/image085.png)


### 5.3.3.3. Cấu hình lifecycle rule cho bucket lưu trữ

1. Chọn bucket cần áp dụng lifecycle.
2. Vào Management → Lifecycle rules.
3. Chọn Create lifecycle rule.
4. Đặt tên ví dụ: `Move-to-IA-after-90d`.
5. Chọn Apply to all objects in the bucket.
6. Chọn chuyển object sang `Standard-IA` sau 90 ngày.

![Hình 87.](/images/5-Workshop/5.3-Neon-Infracstructure/image087.png)

### 5.3.3.4. Chặn truy cập public và bật ACL cho bucket

1. Chọn bucket.
2. Vào tab Permissions.
3. Chọn Block public access → Edit.
4. Bật tất cả tùy chọn Block all public access.
5. Lưu lại.
6. Vào Object Ownership → Edit.
7. Chọn `ACLs enabled`.
8. Chọn `Bucket owner preferred`.
9. Nhấn Save changes.


![Hình 89.](/images/5-Workshop/5.3-Neon-Infracstructure/image089.png)
![Hình 91.](/images/5-Workshop/5.3-Neon-Infracstructure/image091.png)
![Hình 93.](/images/5-Workshop/5.3-Neon-Infracstructure/image093.png)
![Hình 95.](/images/5-Workshop/5.3-Neon-Infracstructure/image095.png)
![Hình 97.](/images/5-Workshop/5.3-Neon-Infracstructure/image097.png)
![Hình 98.](/images/5-Workshop/5.3-Neon-Infracstructure/image098.png)