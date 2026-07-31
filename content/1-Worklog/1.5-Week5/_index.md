---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### What I wanted to get done this week

* Actually measure how good the model is, not just eyeball it.
* Start tracking model versions properly.

### What I did

* Set up Precision@10, Recall@10, and NDCG@10 on the held-out test set.
* Added a "Top Trending" fallback for new users who don't have any ratings yet.
* Registered the model in SageMaker Model Registry.
* Wrote up a short model card — dataset version, hyperparameters, results.

### How it went

* Numbers show the model actually beats a plain trending baseline, which was a relief.
* Cold-start case is handled now, at least in a basic way.
* First model version is registered and tracked properly instead of just sitting on S3 with no context.
