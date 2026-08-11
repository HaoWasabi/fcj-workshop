---
title: "Week 5 Worklog"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

Complete Task CI-001: GitHub Actions CI/CD pipeline according to the Sprint 2 plan.
- Workflow is triggered on push to the main branch
- OIDC authentication works (no hardcoded keys in source code)
- Docker image is built and pushed to ECR
- Tests are executed and pass
- Approval required for the main branch

### Tasks to be carried out this week:

| No. | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - Receive and analyze Sprint 2 requirements <br> - Study the Acceptance Criteria for CI-001 | 18/07/2026 | 19/07/2026 | <https://aws.amazon.com/ecr/> |
| 2 | - Create the GitHub Actions workflow file (.github/workflows/deploy.yml) <br> - Configure OIDC authentication (GitHub → AWS) | 18/07/2026 | 19/07/2026 | <https://docs.github.com/en/actions/> |
| 3 | - Add Docker build step (backend + frontend) <br> - Add push step to ECR <br> - Add unit test step (Django pytest) <br> - Add E2E test step for frontend (Playwright) | 19/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Test the workflow on a feature branch <br> - Set up approval requirements for the main branch <br> - Add a smoke test step after deployment | 19/07/2026 | 20/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Finalize deployment documentation <br> - Prepare for Sprint 3 | 20/07/2026 | 21/07/2026 | |

### Week 5 Achievements:

- Completed the deployment and configuration of Task CI-001 according to the Sprint 2 requirements.
- Gained a clear understanding of the deployment process and AWS resource configuration related to the task.
- Successfully tested and confirmed that resources operate according to the Acceptance Criteria.
- Finalized the deployment guide documentation and recorded issues encountered during implementation.
- Ready to move on to the next sprint tasks after completing CI-001.
