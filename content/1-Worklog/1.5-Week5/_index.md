---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Objectives

* Establish a quantitative way to evaluate model quality.
* Start tracking model versions formally.

### What I did

* Implemented Precision@10, Recall@10, and NDCG@10 on the held-out test set.
* Added a "Top Trending" fallback to handle new users with no rating history.
* Registered the model in SageMaker Model Registry.
* Documented a short model card covering dataset version, hyperparameters, and evaluation results.

### Results

* Confirmed that the model outperforms a trending-only baseline.
* Implemented a basic but working cold-start strategy.
* The first model version is registered and tracked in the Model Registry.
