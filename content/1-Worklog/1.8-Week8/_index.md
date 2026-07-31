---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
Haven't gotten here yet — will update this with what actually happened once it's done.
{{% /notice %}}

### What I want to get done this week

* Stop retraining by hand — automate it.

### Plan

* Build a SageMaker Pipeline chaining Processing → Training → Evaluation.
* Set up an EventBridge rule to trigger it on a schedule.
* Add a check so a new model only gets promoted if it actually beats the current one.

### What I'm hoping for

* A retraining pipeline that runs on its own.
* Some kind of guardrail so a worse model never makes it to production by accident.
