---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### What I wanted to get done this week

* Get SageMaker Studio set up as my main workspace.
* Build a preprocessing step I can rerun instead of doing it by hand each time.

### What I did

* Took "Amazon SageMaker Studio for Data Scientists" on Skill Builder.
* Set up a SageMaker Studio domain and profile in ap-southeast-2.
* Built a SageMaker Processing job to clean the MovieLens data and build the user-item matrix.
* Split train/test by time instead of randomly — makes more sense for a recommender.
* Wrote the processed data back to S3 under `processed/`.

### How it went

* First Processing job ran end-to-end without me babysitting it.
* Got a pipeline I can rerun any time and get a training-ready dataset out.
* Understand now why chronological splitting actually matters here.
