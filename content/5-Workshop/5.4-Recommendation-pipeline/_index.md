---
title: "Recommendation Pipeline"
date: 2026-07-30
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

The pipeline supports multiple strategies to serve users based on their current interaction state.

## Recommendation Strategies

- **Popularity:** IMDb-style weighted popularity ranking for unauthenticated guests.
- **Content-Based:** Uses TF-IDF and cosine similarity for new users based on onboarding genre selections.
- **Collaborative Filtering:** Employs implicit ALS to generate recommendation candidates for returning users.
- **Hybrid Ranking:** Combines weighted Reciprocal Rank Fusion (RRF), recent similarity signals, and business-rule filtering.

{{< mermaid align="center" >}}
flowchart LR
    Input[S3 processed/training/exports] --> Job[Local retrain or SageMaker Processing]
    Job --> ALS[Implicit ALS]
    Job --> Eval[Offline Evaluation]
    Eval --> Gate[Promotion Gate]
    Gate --> Artifacts[Versioned Artifacts]
    Artifacts --> Latest[LATEST.json]
    Artifacts --> S3[(S3 models/evaluation)]
    Latest -. Requires serving package .-> Endpoint[SageMaker Endpoint]
    Endpoint --> API[FastAPI Provider]
{{< /mermaid >}}

<!-- IMAGE-5.4-01: Training, evaluation, promotion, and serving workflow diagram. -->

## Inputs and Outputs

Pipeline inputs include:

- Movie feature artifacts.
- Temporally split interaction logs.
- Onboarding genre selections.
- Recent user interactions.
- List of movie IDs to exclude.

The recommendation engine outputs:

- An ordered list of `movie_id` references.
- `score` values.
- `reason_code` tags.
- `reason_context` metadata.

The backend enriches responses with metadata from the `Movies` table and caches `movie_id`, `score`, and `reason_code`.

## Roles of SageMaker and EC2

SageMaker Processing Jobs execute containerized model retraining. EC2 hosts Docker application containers and optionally runs scheduled retraining via systemd.

The backend includes a client interface to invoke a SageMaker real-time endpoint, but the current repository does not contain endpoint packaging or automated deployment scripts.

{{% notice warning %}}
Processing Jobs, local inference engines, and real-time endpoints represent three distinct components. Do not describe Processing Jobs as SageMaker Training Jobs or real-time endpoints.
{{% /notice %}}

## Table of Contents

1. [Prepare Recommendation Environment](5.4.1-prepare-environment/)
2. [Train, Evaluate, and Run SageMaker Processing Job](5.4.2-train-and-deploy-model/)
3. [Integrate Application on EC2](5.4.3-integrate-ec2-application/)
4. [End-to-End Testing](5.4.4-end-to-end-testing/)

**Reference Sources:** `ml/README.md`, `ml/configs/model_serving.yaml`, `ml/scripts/sagemaker_retrain_job.py`, and backend recommendation providers.
