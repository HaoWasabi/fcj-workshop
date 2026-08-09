---
title : "Chuẩn bị mã nguồn và cấu trúc dự án"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

### 5.4.1.1. Chuẩn bị mã nguồn và cấu trúc dự án

Đảm bảo dự án đã có mã nguồn chạy được và tuân thủ cấu trúc thư mục tiêu chuẩn sau:

```text
NeonFoodmap/
├── .github/
│   └── workflows/
│       └── deploy.yml              ← File pipeline chính
├── backend/
│   ├── Dockerfile                  ← Đóng gói backend thành Docker image
│   ├── requirements.txt            ← Dependencies Python
│   ├── config/
│   │   ├── settings.py             ← Settings production
│   │   └── settings_test.py        ← Settings cho CI test (SQLite in-memory)
│   └── manage.py
├── frontend/
│   ├── Dockerfile                  ← Đóng gói frontend thành Docker image
│   ├── package.json                ← Scripts: lint, build, test
│   └── playwright.config.ts        ← Cấu hình E2E tests
└── neonfoodmap-iam-setup.yaml      ← CloudFormation template cho IAM
```

### 5.4.1.2. Các kiểm tra bắt buộc trước khi tiếp tục:

- `backend/Dockerfile` tồn tại và build được locally (`docker build -t test ./backend`)
- `frontend/Dockerfile` tồn tại và build được locally
- `backend/config/settings_test.py` dùng SQLite in-memory (không cần cơ sở dữ liệu thật khi test)
- Frontend có khai báo các scripts: `npm run lint`, `npm run build`
- File `.github/workflows/deploy.yml` đã có trong kho lưu trữ GitHub

### 5.4.1.3. Lấy file Workflow GitHub Actions:

Vào trang GitHub của nhóm, truy cập `.github/workflows` và tải file `deploy.yml`:
- **URL tham chiếu**: [https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml](https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml)
