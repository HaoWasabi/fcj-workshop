---
title: "Blog 4"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

# BUYING SAVINGS PLANS BEFORE RIGHTSIZING CAN MAKE YOU OPTIMIZE COSTS IN THE WRONG ORDER

When EC2 bills start climbing, the instinctive response for many teams is to **buy Savings Plans as soon as possible.**

This reduces the per-unit compute price, but **doesn't necessarily make the system truly efficient.**

If the workload is still over-provisioned, you're simply paying a better rate for resources you didn't need in the first place.

*On July 16, 2026, AWS added the Cost Efficiency widget to the Billing and Cost Management Dashboards. This widget lets you track optimization levels over time, per AWS account or Region, and links directly to Cost Optimization Hub to act on savings opportunities.*

The real takeaway isn't that AWS added a new chart — it's how AWS defines "cost optimization."

## COST EFFICIENCY IS NOT "HOW MUCH DID WE SAVE THIS MONTH"

AWS calculates Cost Efficiency using this formula:
```
Cost Efficiency
= [1 − Potential Savings / Total Optimizable Spend] × 100%
```

For example, if an organization has $100,000 in optimizable spend and Cost Optimization Hub identifies $10,000 in savings opportunities, Cost Efficiency would be 90%.

The metric is updated daily, based on the last 30 days of costs and existing savings opportunities. It aggregates:
– Idle resources
– Rightsizing
– Savings Plans and Reserved Instances
– Right instance type selection
– Optimization recommendations across EC2, ECS, EKS, EBS, RDS, Lambda, DynamoDB, OpenSearch, and more

## 3 KEY LESSONS FOR AWS COST OPTIMIZATION

### 1. RIGHTSIZE FIRST, COMMIT LATER

The correct order should be:
Remove idle resources
→ Rightsize workloads
→ Migrate to newer instance generations or Graviton where appropriate
→ Then purchase Savings Plans or Reserved Instances.

If you commit first, you may lock in spend on an over-provisioned workload. When you rightsize later, the freed-up capacity may not reduce your bill proportionally because the commitment is still in effect.

AWS also found that large customers who combined rightsizing with Savings Plans improved Cost Efficiency approximately four times faster than those who relied primarily on Savings Plans alone.

### 2. WITHOUT MEMORY METRICS, EC2 RIGHTSIZING IS FLYING BLIND

CloudWatch collects many EC2 metrics by default, but memory utilization data from inside the operating system is not collected automatically.

Without memory data, Compute Optimizer may lack sufficient information to determine whether an instance is over-provisioned on RAM or to recommend a smaller configuration.

In an analysis of over 71,000 opted-in AWS customers, only 17.7% of eligible workloads had memory metrics enabled. Having this data was associated with 8 to 30 percentage points higher savings per recommendation, depending on the instance type.

So before concluding that an EC2 instance has "nothing left to optimize," verify that CloudWatch Agent or your observability tooling is actually sending memory metrics.

### 3. DON'T TREAT COST OPTIMIZATION AS A QUARTERLY CAMPAIGN

Cost Efficiency updates daily and can now be placed alongside Cost Explorer, Budgets, Savings Plans coverage, and Reserved Instance utilization on a unified dashboard.

The dashboard also supports cross-account sharing, CSV/PDF export, and scheduled email reports.

A practical workflow could be:
- Weekly: check which accounts or Regions show a significant drop in Cost Efficiency.
- Monthly: review high-savings, low-effort recommendations with rollback options.
- Quarterly: adjust commitments based on rightsized workloads, not just current spend levels.

## CONCLUSION

Savings Plans don't replace architectural optimization.
Rightsizing doesn't replace commitment discounts either.

A solid FinOps strategy requires doing things in the right order:
Eliminate waste → optimize resources → stabilize workloads → then commit spend.

AWS's new Cost Efficiency widget helps turn this process from a scattered list of recommendations into a measurable feedback loop over time.

## References

[1]: **Cost Efficiency widget in Billing and Cost Management Dashboards** — AWS, July 16, 2026.<br>
https://aws.amazon.com/about-aws/whats-new/2026/07/monitor-cost-efficiency-using-dashboards

[2]: **The AWS State of Cost Efficiency Report** — AWS Cloud Financial Management, June 09, 2026.<br>
https://aws.amazon.com/blogs/aws-cloud-financial-management/the-aws-state-of-cost-efficiency-report/

[3]: **Understanding your cost efficiency metric** — AWS Cost Management Documentation.<br>
https://docs.aws.amazon.com/cost-management/latest/userguide/coh-cost-efficiency.html

Ho Chi Minh City, August 04, 2026 <br>
Bui Bao Long

[Link to post at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2234324983999128/)
