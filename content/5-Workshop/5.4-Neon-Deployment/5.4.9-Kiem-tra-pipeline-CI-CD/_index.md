---
title : "Verify CI/CD Pipeline"
date : 2024-01-01
weight : 9
chapter : false
pre : " <b> 5.4.9. </b> "
---

After completing the GitHub Actions configuration, go to **GitHub → Actions** to monitor the pipeline execution. The pipeline is designed with **6 jobs** that run sequentially to test the source code, build Docker images, and deploy the application to AWS.

| Order | Job | Purpose |
|-------|-----|---------|
| 1 | `backend-test` | Code quality check and Backend Unit Tests |
| 2 | `frontend-check` | Frontend lint check and application build |
| 3 | `e2e-tests` | Run Playwright End-to-End Tests |
| 4 | `build-and-push` | Build Docker Image and push to Amazon ECR |
| 5 | `deploy-backend` | Deploy the new version to Amazon ECS |
| 6 | `smoke-tests` | Verify service status after Deploy |

---

### 5.4.9.1 Pipeline Triggers

| Event | Branch | Jobs Executed |
|-------|--------|---------------|
| `push` | `main` | All 6 jobs |
| `push` | `develop`, `feature/**` | `backend-test`, `frontend-check`, `e2e-tests` |
| `pull_request` | `main` | `backend-test`, `frontend-check`, `e2e-tests` |

---

### 5.4.9.2. Job Details

#### Job 1 – `backend-test`

| Field | Value |
|-------|-------|
| Purpose | Check backend source code quality |
| Runner | `ubuntu-latest` + Python 3.12 |
| Executes | `flake8` and `python manage.py test` |
| PASS Criteria | No lint errors and all Unit Tests succeed |

---

#### Job 2 – `frontend-check`

| Field | Value |
|-------|-------|
| Purpose | Lint check and build the frontend |
| Runner | `ubuntu-latest` + Node.js 22 |
| Executes | `npm run lint`, `npm run build` |
| PASS Criteria | Build succeeds with no ESLint errors |

---

#### Job 3 – `e2e-tests`

| Field | Value |
|-------|-------|
| Purpose | Test critical application features |
| Tool | Playwright (Chromium) |
| Condition | `frontend-check` must pass |
| Output | Test report stored in GitHub Actions |

---

#### Job 4 – `build-and-push`

| Field | Value |
|-------|-------|
| Purpose | Build Docker Image and push to Amazon ECR |
| Condition | Both `backend-test` and `e2e-tests` must pass |
| Only runs | On push to `main` branch |
| Auth | GitHub OIDC Assume IAM Role |

---

#### Job 5 – `deploy-backend`

| Field | Value |
|-------|-------|
| Purpose | Deploy the new version to Amazon ECS |
| Strategy | Rolling Update |
| Migration | Executed via ECS Run Task |
| PASS Criteria | Service updated successfully |

---

#### Job 6 – `smoke-tests`

| Field | Value |
|-------|-------|
| Purpose | Verify service status after Deploy |
| Endpoints | `/api/`, `/api/pois/`, `/api/tours/`, `/api/categories/` |
| PASS Criteria | HTTP Status < 500 |

---

### 5.4.9.3 Pipeline Authentication and Security

The pipeline uses **GitHub OIDC Federation** to authenticate with AWS via an **IAM Role** instead of storing static Access Keys in GitHub Secrets. This improves security and only allows the specified repository to perform deployments.

| Component | Description |
|-----------|-------------|
| OIDC Federation | Authentication using temporary tokens |
| Least Privilege | IAM Role grants only necessary permissions for ECR and ECS |
| Environment Protection | Manual Approval can be required before deploying to Production |
| Secret Management | Sensitive information stored in GitHub Secrets |
| Branch Protection | Only the `main` branch can trigger deployments |

---

### 5.4.9.4. Common Troubleshooting

| Issue | Cause | Resolution |
|-------|-------|------------|
| OIDC login failed | Incorrect Role ARN or Trust Policy | Check `AWS_ROLE_ARN` and OIDC configuration |
| Backend test fail | Source code error or failed Unit Tests | Review GitHub Actions log and run tests locally |
| Frontend build fail | Dependency error or TypeScript issue | Run `npm ci` and `npm run build` |
| ECS deploy timeout | Container failed to start | Check CloudWatch Logs and environment variables |
| Smoke test fail | Service not ready or wrong URL | Check Health Check and application Endpoints |
