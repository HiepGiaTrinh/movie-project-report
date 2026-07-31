---
title: "Week 3 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---

### Objectives

* Set up SageMaker Studio as the primary workspace.
* Build a repeatable preprocessing step instead of relying on manual work.

### What I did

* Completed "Amazon SageMaker Studio for Data Scientists" on AWS Skill Builder.
* Set up a SageMaker Studio domain and user profile in ap-southeast-2.
* Built a SageMaker Processing job to clean the MovieLens dataset and construct the user-item matrix.
* Implemented a time-based train/test split instead of a random split, which better reflects the sequential nature of a recommendation system.
* Wrote the processed data back to S3 under the `processed/` prefix.

### Results

* The first Processing job ran successfully end-to-end.
* Established a reproducible pipeline that produces a training-ready dataset.
* Gained a clearer understanding of why chronological splitting matters for this type of system.
