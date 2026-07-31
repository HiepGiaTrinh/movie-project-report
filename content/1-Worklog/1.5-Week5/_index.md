---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---

### Week 5 Objectives:

* Quantitatively evaluate the recommendation model.
* Track model versions with the SageMaker Model Registry.

### Tasks:

* Implemented evaluation metrics for the recommender: **Precision@10**, **Recall@10**, and **NDCG@10** on the held-out test set.
* Added a simple **"Top Trending"** fallback to handle the cold-start problem for new users with no rating history.
* Registered the trained model version in the **SageMaker Model Registry** to track lineage and approval status.
* Documented a model card: dataset version, hyperparameters, and evaluation results.

### Week 5 Achievements:

* Quantitative evaluation confirming the model outperforms a trending-only baseline.
* A working cold-start fallback strategy, tested with simulated new users.
* First model version approved and tracked in the SageMaker Model Registry.
