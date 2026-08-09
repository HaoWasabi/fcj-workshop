---
title : "Configure GitHub Actions Workflow"
date : 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

## Fetch and Configure GitHub Actions Workflow

The project's CI/CD pipeline is fully automated using GitHub Actions. The workflow configuration file orchestrates tasks ranging from code testing, container image packaging, pushing to AWS ECR, and automated deployment to Amazon ECS.

### 5.4.2.1. Retrieve the Workflow File
Access the project's GitHub repository to view/download the workflow file:
- **Path**: `.github/workflows/deploy.yml`
- **Reference Link**: [GitHub Actions deploy.yml](https://github.com/HaoWasabi/NeonFoodmap/blob/main/.github/workflows/deploy.yml)

---

### 5.4.2.2. Pipeline Job Breakdown

The pipeline consists of 6 core sequential/parallel Jobs:

#### Job 1: `backend-test` — Backend Lint & Unit Test
| Item | Details |
| :--- | :--- |
| **Trigger** | Any Push/PR modifying source files in `backend/**` |
| **Runner** | `ubuntu-latest` |
| **Environment** | Python 3.12 (with pip caching) |
| **Linting** | `flake8` checks syntax (critical errors E9, F63, F7, F82 block the pipeline) |
| **Testing** | `python manage.py test --settings=config.settings_test` |
| **Database** | Uses SQLite in-memory (no live MySQL connection required) |

#### Job 2: `frontend-check` — Frontend Lint & Build
| Item | Details |
| :--- | :--- |
| **Trigger** | Any Push/PR modifying source files in `frontend/**` |
| **Runner** | `ubuntu-latest` |
| **Environment** | Node.js 22 (with npm caching) |
| **Linting** | `npm run lint` (ESLint) |
| **Building** | `npm run build` (Vite production build) |

#### Job 3: `e2e-tests` — Playwright E2E Tests
| Item | Details |
| :--- | :--- |
| **Dependency** | Requires `frontend-check` to succeed |
| **Browser** | Chromium (Playwright headless) |
| **Scope** | Executes critical user journey test suites |
| **Artifact** | Test reports stored for 7 days on GitHub Actions |

#### Job 4: `build-and-push` — Build & Push Docker Images
| Item | Details |
| :--- | :--- |
| **Dependency** | Requires both `backend-test` and `e2e-tests` to succeed |
| **Condition** | Runs only on direct push or merge into `main` branch |
| **AWS Auth** | Uses OpenID Connect (OIDC) to assume role `AWS_ROLE_ARN` |
| **Tagging** | Applies `latest` and `sha-<7_commit_chars>` tags |
| **Caching** | Utilizes GitHub Actions Cache to accelerate build times |

#### Job 5: `deploy-backend` — Deploy to Amazon ECS
| Item | Details |
| :--- | :--- |
| **Dependency** | Requires `build-and-push` job completion |
| **Environment** | `production` (Supports manual approval rules) |
| **Strategy** | Rolling Update (ECS manages zero-downtime traffic shift) |
| **Migration** | Executes one-off Fargate task via `run-task` to run DB migrations |

#### Job 6: `smoke-tests` — Post-Deployment Verification
| Item | Details |
| :--- | :--- |
| **Dependency** | Requires `deploy-backend` completion |
| **Health Check** | Queries API endpoints: `/api/`, `/api/pois/`, `/api/tours/`, `/api/categories/` |
| **PASS Criteria** | HTTP response status code < 500 |

---

### 5.4.2.3. Workflow Triggers Summary Table

| Event | Target Branches | Executed Jobs |
| :--- | :--- | :--- |
| **Push** | `main` | All 6 jobs (Test → Build → Deploy → Smoke Tests) |
| **Push** | `develop`, `feature/*` | Only 3 test jobs: `backend-test`, `frontend-check`, `e2e-tests` |
| **Pull Request** | Target `main` | Only 3 test jobs: `backend-test`, `frontend-check`, `e2e-tests` |

---

### 5.4.2.4. Pipeline Security Architecture

| Security Control | Implementation Details |
| :--- | :--- |
| **OIDC Federation** | GitHub Actions authenticates with AWS via ephemeral STS tokens; no static keys stored |
| **Least Privilege** | GitHub Actions Role permissions scoped strictly to ECR push and ECS service update |
| **Environment Protection** | Production deployment job requires explicit reviewer approval |
| **Docker Non-root** | Container process runs as `appuser` unprivileged account |
| **Multi-stage Build** | Production image contains only runtime binaries; build tools stripped |
| **Secret Management** | All credentials injected via GitHub Secrets, zero hardcoded secrets |
| **Branch Protection** | Deployments triggered strictly on verified merges into `main` |
