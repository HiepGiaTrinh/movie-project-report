---
title: "Week 8 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---

{{% notice note %}}
This is a planned week. Content will be updated with actual results once the work below is completed.
{{% /notice %}}

### Week 8 Objectives:

* Automate the retraining process so the model can improve without manual intervention.

### Planned Tasks:

* Build a **SageMaker Pipeline** that chains Processing → Training → Evaluation steps for automated retraining.
* Configure an **EventBridge** rule to trigger the retraining pipeline on a schedule.
* Add a conditional "approval" step so a new model is only promoted if it meets the minimum evaluation threshold.

### Expected Outcomes:

* A working, schedulable retraining pipeline that removes the need for manual retraining.
* Clear promotion criteria so only better-performing models reach production.
