---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
This week is still in progress at the time of writing. The results below reflect what has been completed so far.
{{% /notice %}}

### Objectives

* Integrate the endpoint with the full application and test it end-to-end.
* Begin designing the feedback loop.

### What I did

* Connected the SageMaker endpoint to the team's EC2-hosted application and ran a full end-to-end test covering login, browsing, and receiving recommendations.
* Resolved several integration issues between the backend and the endpoint, including payload formatting and error handling.
* Began designing the feedback loop: new ratings are written to DynamoDB and exported to S3 for retraining.
* Drafted the retraining flow: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### Results (in progress)

* Achieved a working end-to-end demo alongside the rest of the team's system.
* Have an initial design for the feedback loop, ready to implement next week.
