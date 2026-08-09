---
title : "Kích hoạt pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10. </b> "
---

### 5.4.10.1. Kích hoạt pipeline

Sau khi hoàn tất cấu hình GitHub Actions và xác nhận workflow đã được lưu trong repository, thực hiện đẩy mã nguồn lên nhánh `main` để kích hoạt quy trình CI/CD. Khi có sự kiện **push** vào nhánh `main`, GitHub Actions sẽ tự động thực thi toàn bộ pipeline gồm các bước kiểm tra mã nguồn, xây dựng Docker Image, triển khai lên Amazon ECS và kiểm tra trạng thái ứng dụng sau khi triển khai.

#### 5.4.10.2. Chuyển sang nhánh `main`

Mở Terminal tại thư mục dự án và chuyển sang nhánh `main`.

```bash
git checkout main
```

> Nếu nhánh `main` chưa cập nhật phiên bản mới nhất từ GitHub, nên đồng bộ trước khi thực hiện merge.

```bash
git pull origin main
```

---

#### 5.4.10.3. Hợp nhất mã nguồn từ nhánh `develop`

Thực hiện merge toàn bộ thay đổi từ nhánh `develop` vào `main`.

```bash
git merge develop
```

Nếu xảy ra **Merge Conflict**, cần chỉnh sửa các tệp bị xung đột, sau đó tiếp tục quá trình merge.

```bash
git add .
git commit
```

Khi merge thành công, toàn bộ thay đổi trên nhánh `develop` sẽ được đưa vào nhánh `main`.

---

#### 5.4.10.4. Đẩy mã nguồn lên GitHub

Thực hiện push nhánh `main` lên GitHub.

```bash
git push origin main
```

Ngay sau khi lệnh hoàn thành, GitHub sẽ phát sinh sự kiện **push**, từ đó tự động kích hoạt workflow được định nghĩa trong thư mục `.github/workflows`.

---

#### 5.4.10.5. Theo dõi quá trình thực thi Pipeline

Truy cập repository trên GitHub, chọn **Actions** để theo dõi trạng thái thực thi của pipeline.

Pipeline sẽ thực hiện tuần tự các job:

1. `backend-test`
2. `frontend-check`
3. `e2e-tests`
4. `build-and-push`
5. `deploy-backend`
6. `smoke-tests`

Mỗi job chỉ được thực hiện khi job trước đó hoàn thành thành công. Nếu một job thất bại, các bước phụ thuộc phía sau sẽ không được thực hiện.

---

#### 5.4.10.6. Kiểm tra kết quả

Khi pipeline hoàn tất, kiểm tra các nội dung sau:

| Nội dung | Kết quả mong đợi |
|----------|------------------|
| Backend Test | Thành công |
| Frontend Build | Thành công |
| Playwright E2E | Thành công |
| Docker Image | Được đẩy lên Amazon ECR |
| ECS Deployment | Service cập nhật phiên bản mới |
| Smoke Test | Các API trả về HTTP Status hợp lệ |

Nếu tất cả các job đều hiển thị trạng thái **Success**, quá trình CI/CD đã được kích hoạt và triển khai thành công.

---

#### Ví dụ

```bash
git checkout main
git pull origin main

git merge develop

git push origin main
```