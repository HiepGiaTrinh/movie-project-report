---
title: "Week 9 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---

{{% notice note %}}
Haven't gotten here yet — will update this with what actually happened once it's done.
{{% /notice %}}

### What I want to get done this week

* Actually know if the model is doing okay, instead of just hoping.

### Plan

* Set up SageMaker Model Monitor for data drift on the live endpoint.
* Build CloudWatch dashboards/alarms for invocations, latency, errors.
* Wire up SNS so I get notified if something breaks.

### What I'm hoping for

* Some visibility into model health instead of flying blind.
* Get pinged before users notice something's wrong, not after.
