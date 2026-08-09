---
title : "Create GitHub Environment 'production'"
date : 2024-01-01
weight : 6
chapter : false
pre : " <b> 5.4.6. </b> "
---

GitHub Environments provide deployment protection rules—such as mandatory manual approval gates—to control software releases into production infrastructure.

---

### 5.4.6.1. Step-by-Step Instructions:

1. **Initialize Environment**:
   - Navigate to your project's GitHub repository.
   - Go to **Settings** → select **Environments** in the left sidebar.
   - Click the **New environment** button.

2. **Configure Environment Name**:
   - Enter the exact name: `production`.
   - Click **Configure environment**.

3. **Configure Required Reviewers Protection Rule**:
   - Under **Deployment protection rules**, check **Required reviewers**.
   - Search for and select 1–2 designated team reviewers (e.g., Lead DevOps / Tech Lead).
   - When the pipeline executes the `deploy-backend` job, GitHub halts deployment and notifies reviewers for manual approval.

4. **(Optional) Configure Wait Timer**:
   - To enforce a mandatory soak time before deployment proceeds, enable **Wait timer** (e.g., `5` minutes).
   - Click **Save protection rules** to finalize.
