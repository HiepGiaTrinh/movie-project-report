---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
This week is still in progress at the time of writing — the achievements below reflect the work completed so far.
{{% /notice %}}

### Week 7 Objectives:

* Integrate the SageMaker endpoint with the team's application end-to-end.
* Design the feedback loop and retraining pipeline.

### Tasks:

* Integrated the SageMaker endpoint with the team's EC2-hosted application and ran a full end-to-end test: user login → browse movies → receive recommendations.
* Reviewed logs and fixed integration issues between the backend and the ML endpoint (payload format, error handling).
* Started designing the feedback loop: capturing new user ratings in DynamoDB and exporting them to S3 for retraining.
* Drafted the retraining architecture: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### Week 7 Achievements (in progress):

* Successful end-to-end demo of the recommendation flow together with the rest of the team's system.
* Clear design for the feedback loop and retraining pipeline, ready to implement next week.
