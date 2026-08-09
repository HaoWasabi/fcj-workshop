---
title : "Prepare Source Code and Project Structure"
date : 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

### 5.4.1.1. Prepare Source Code and Project Structure

Ensure the project contains functional source code and adheres to the following standard directory structure:

```text
NeonFoodmap/
├── .github/
│   └── workflows/
│       └── deploy.yml              ← Main pipeline workflow file
├── backend/
│   ├── Dockerfile                  ← Containerize backend into Docker image
│   ├── requirements.txt            ← Python dependencies
│   ├── config/
│   │   ├── settings.py             ← Production settings
│   │   └── settings_test.py        ← CI testing settings (SQLite in-memory)
│   └── manage.py
├── frontend/
│   ├── Dockerfile                  ← Containerize frontend into Docker image
│   ├── package.json                ← Scripts: lint, build, test
│   └── playwright.config.ts        ← E2E testing configuration
└── neonfoodmap-iam-setup.yaml      ← CloudFormation template for IAM
```

### 5.4.1.2. Verification Checklist Before Proceeding:

- File `backend/Dockerfile` exists and builds cleanly locally (`docker build -t test ./backend`)
- File `frontend/Dockerfile` exists and builds cleanly locally
- `backend/config/settings_test.py` uses SQLite in-memory (no live database required during test runs)
- Frontend defines scripts: `npm run lint`, `npm run build`
- File `.github/workflows/deploy.yml` is present in the GitHub repository

### 5.4.1.3. Retrieve the GitHub Actions Workflow File:

Access the team's GitHub repository, navigate to `.github/workflows`, and download `deploy.yml`:
- **Reference URL**: [https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml](https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml)
