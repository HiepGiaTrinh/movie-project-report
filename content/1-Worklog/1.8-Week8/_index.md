---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
This week has not started at the time of writing. This section will be updated with actual results once completed.
{{% /notice %}}

### Objectives

* Move from manual retraining to an automated pipeline.

### Plan

* Build a SageMaker Pipeline that chains Processing, Training, and Evaluation steps.
* Configure an EventBridge rule to trigger retraining on a schedule.
* Add a promotion gate so a new model is only deployed if it outperforms the current one.

### Expected Outcomes

* An automated, schedulable retraining pipeline.
* A safeguard to prevent a lower-performing model from reaching production.
