---
title: "Week 2 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### What I wanted to get done this week

* Get comfortable working with data on AWS.
* Set up storage and pull in the dataset I'll actually be training on.

### What I did

* Took "AWS Technical Essentials" and "Introduction to IAM" on Skill Builder.
* Set up an S3 bucket with `raw/`, `processed/`, `models/`, `logs/` folders.
* Wrote a least-privilege IAM policy and role just for the data-processing work.
* Downloaded MovieLens 100K, uploaded it to `raw/`.
* Poked around the data locally with pandas — rating distribution, how many users/items, how sparse the matrix is.

### How it went

* Bucket's set up and has the raw MovieLens data in it.
* IAM role works and isn't over-permissioned.
* Got a feel for the data, which helped me figure out how to preprocess it next.
