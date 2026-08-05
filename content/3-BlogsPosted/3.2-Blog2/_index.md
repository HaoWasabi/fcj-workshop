---
title: "Blog 2"
date: 2026-07-31
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# AMAZON RDS AUTOMATION ON SCHEDULE

Hi everyone, I'm researching how to optimize costs on AWS and came across a tutorial on **"Automatically stop and start an Amazon RDS DB instance using AWS Systems Manager Maintenance Windows"** in AWS Prescriptive Guidance.

The article shares how to **automatically turn Amazon RDS on and off** according to a schedule, for example, only starting the database during working hours and turning it off in the evening or on weekends. This method is quite suitable for development, testing, or staging environments that don't need to operate 24/7.

Initially, I thought I would have to use EventBridge in combination with Lambda and write my own code to call the RDS API. However, AWS Systems Manager already has two built-in Automation runbooks, `AWS-StartRdsInstance` and `AWS-StopRdsInstance`, so almost no additional logic is needed.

Key points to remember:

* Create two Maintenance Windows for the start and stop schedules.

* Use cron expressions to define the run times.

* Assign tags to the RDS instances that need to be affected.

* Group the instances into Resource Groups.

* Use an Automation runbook to automatically start or stop the database.

The best part is that when there are many RDS instances, I only need to assign the same tag to them instead of manually configuring each database. The Systems Manager will perform the task on all resources in the group.

Some takeaways:

* Not all resources need to run 24/7, especially in development and test environments.

* Before writing Lambda for automation, check if AWS already has a suitable runbook.

* The IAM role should only be granted permission to start and stop the necessary RDS instances.

* RDS can only be stopped continuously for a maximum of 7 days, after which AWS will automatically restart for maintenance.

My project currently keeps the database running continuously and only shuts it down when not in use, so this article has shown me a fairly simple way to optimize costs without building too many additional components.

Thank you for reading my sharing.

I'm still learning, so if there's anything I don't understand, please feel free to provide feedback.

Link to the reference article: https://docs.aws.amazon.com/prescriptive-guidance/latest/patterns/automatically-stop-and-start-an-amazon-rds-db-instance-using-aws-systems-manager-maintenance-windows.html

Ho Chi Minh City, July 31, 2026 <br>
Diep Thuy An

![Illustrative Image](/images/blog2.png)

[Blog link at AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2230171591081134/)