---
title : "Trigger Pipeline"
date : 2024-01-01
weight : 10
chapter : false
pre : " <b> 5.4.10. </b> "
---

After completing the GitHub Actions configuration and confirming the workflow has been saved in the repository, push the source code to the `main` branch to trigger the CI/CD process. When a **push** event occurs on the `main` branch, GitHub Actions automatically executes the entire pipeline — including source code checks, Docker image builds, deployment to Amazon ECS, and post-deployment status verification.

### 5.4.10.1. Switch to the `main` Branch

Open a Terminal in the project directory and switch to the `main` branch.

```bash
git checkout main
```

> If the `main` branch is not up to date with the latest version from GitHub, sync it first before proceeding.

```bash
git pull origin main
```

---

### 5.4.10.2. Merge Source Code from the `develop` Branch

Merge all changes from the `develop` branch into `main`.

```bash
git merge develop
```

If a **Merge Conflict** occurs, edit the conflicting files and then continue the merge process.

```bash
git add .
git commit
```

Once the merge is successful, all changes from the `develop` branch will be incorporated into `main`.

---

### 5.4.10.3. Push the Source Code to GitHub

Push the `main` branch to GitHub.

```bash
git push origin main
```

As soon as the command completes, GitHub generates a **push** event, which automatically triggers the workflow defined in the `.github/workflows` directory.

---

### 5.4.10.4. Monitor Pipeline Execution

Go to your GitHub repository and select **Actions** to monitor the pipeline execution status.

The pipeline runs the following jobs in sequence:

1. `backend-test`
2. `frontend-check`
3. `e2e-tests`
4. `build-and-push`
5. `deploy-backend`
6. `smoke-tests`

Each job only runs after the previous one completes successfully. If a job fails, all dependent downstream steps will not be executed.

---

### 5.4.10.5. Verify Results

When the pipeline finishes, check the following:

| Item | Expected Result |
|------|----------------|
| Backend Test | Passed |
| Frontend Build | Passed |
| Playwright E2E | Passed |
| Docker Image | Pushed to Amazon ECR |
| ECS Deployment | Service updated to the new version |
| Smoke Test | All APIs return valid HTTP Status codes |

If all jobs show a **Success** status, the CI/CD process has been triggered and deployed successfully.

---

### 5.4.10.Example

```bash
git checkout main
git pull origin main

git merge develop

git push origin main
```
