---
title: "Week 6 Worklog"
date: 2026-07-27
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Week 6 Objectives:

Complete Task AWS-008: Set up CloudFront + CDN according to the Sprint 3 plan.
- CDN domain names are created
- Frontend is distributed via CloudFront
- Audio files are cached globally
- HTTPS works correctly
- Cache hit rate > 80%

Complete Task QA-002: End-to-End Testing according to the Sprint 4 plan.
- All critical user flows work correctly
- No broken links or 404 errors
- Audio playback via CloudFront works correctly
- Error messages are helpful
- Performance is at an acceptable level

### Tasks to be carried out this week:

| No. | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - Receive and analyze Sprint 3 requirements <br> - Study the Acceptance Criteria for AWS-008 | 29/07/2026 | 30/07/2026 | <https://aws.amazon.com/cloudfront/> |
| 2 | - Create CloudFront distributions | 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Configure cache behaviors (*.js: 24h, *.html: 1h) <br> - Set TTL for cache (1 hour for frontend, 24 hours for audio) | 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Set up HTTPS/SSL for CloudFront <br> - Set up Origin Access Identity (OAI) for S3 <br> - Configure compression (gzip, brotli) | 29/07/2026 | 30/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Test CDN performance (latency from Singapore) <br> - Monitor cache hit ratio | 30/07/2026 | 31/07/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Test user registration and login flows <br> - Test the POI browsing and description loading feature <br> - Test audio playback via CloudFront <br> - Test the tour booking flow <br> - Test payment integration (Sandbox environment) <br> - Test error scenarios (invalid data, timeouts) <br> - Test mobile responsiveness | 30/07/2026 | 31/07/2026 | |
| 7 | - Finalize deployment documentation <br> - Prepare for the next sprint | 31/07/2026 | 01/08/2026 | |

### Week 6 Achievements:

- Completed the deployment and configuration of Task AWS-008 according to the Sprint 3 requirements.
- Completed testing for Task QA-002 according to the Sprint 3 requirements.
- Gained a clear understanding of the deployment process and AWS resource configuration related to the tasks.
- Successfully tested and confirmed that resources operate according to the Acceptance Criteria.
- Finalized the deployment guide documentation and recorded issues encountered during implementation.
- Ready to move on to the next sprint tasks after completing AWS-008 and QA-002.
