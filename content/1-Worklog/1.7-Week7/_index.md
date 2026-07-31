---
title: "Week 7 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---

{{% notice note %}}
Still working through this week as I write this — the notes below are what's done so far.
{{% /notice %}}

### What I wanted to get done this week

* Get the endpoint hooked into the full app and test it end-to-end.
* Start figuring out the feedback loop.

### What I did

* Connected the SageMaker endpoint to the team's EC2 app and ran a full test: log in, browse, get recommendations.
* Chased down a few integration bugs between the backend and the endpoint (payload format, error handling — the usual).
* Started sketching out the feedback loop: new ratings go into DynamoDB, get exported to S3 for retraining.
* Drafted the retraining flow on paper: DynamoDB Stream → Lambda → S3 → SageMaker Pipeline.

### How it's going so far

* Got a working end-to-end demo with the rest of the system.
* Have a rough design for the feedback loop, ready to actually build next week.
