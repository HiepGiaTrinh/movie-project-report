---
title: "Verify and Prepare Storage Resources"
date: 2026-07-30
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

The repository does not contain Terraform, CloudFormation, CDK, or equivalent Infrastructure as Code (IaC) tools. Therefore, this section only verifies existing resources provisioned by the platform team and does not automatically provision new resources.

## 1. Verify Five DynamoDB Tables

For each table name provided via secure configuration channels, run:

```bash
aws dynamodb describe-table \
  --table-name "<MOVIES_TABLE_NAME>" \
  --region "<AWS_REGION>"
```

Repeat for the remaining four tables and verify:

| Table | HASH Key | RANGE Key |
|---|---|---|
| `Movies` | `movie_id` | None |
| `PopularMovies` | `list_id` | None |
| `Users` | `user_id` | None |
| `UserInteractions` | `user_id` | `interaction_key` |
| `RecommendationCache` | `user_id` | `scenario` |

All tables must be in the `ACTIVE` status.

![Five DynamoDB tables in Active status](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/dynamodb-tables.png)

*The five DynamoDB tables with their partition keys, sort keys, and Active status.*

## 2. Verify Additional Attributes

The current design does not use GSIs. `RecommendationCache` includes an `expire_at` field, but application code alone does not confirm that TTL is enabled on the live AWS resource.

Attributes requiring verification outside source code:

- TTL configured on the `expire_at` attribute.
- Billing mode and read/write capacity settings.
- Point-in-time recovery (PITR) status.
- Server-side encryption settings.
- Tags and resource ownership tags.

If evidence from the AWS Console or CLI is unavailable, record the state as **unconfirmed** rather than making assumptions.

## 3. Verify S3 Bucket

![S3 bucket for the movie recommendation system](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket.png)

*The S3 bucket used to store datasets, model artifacts, and evaluation reports.*

![S3 bucket overview](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-overview.png)

*The bucket is deployed in `ap-southeast-1`; the overview directly confirms the ARN and Region of the resource being verified.*

```bash
aws s3api head-bucket \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-public-access-block \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-bucket-encryption \
  --bucket "<S3_BUCKET_NAME>"

aws s3api get-bucket-versioning \
  --bucket "<S3_BUCKET_NAME>"
```

The bucket must exist, enforce Block Public Access, and have appropriate default encryption enabled. For the inspected bucket, Block Public Access is on, default encryption uses SSE-S3, and versioning is `Enabled`. Application source code does not provision these settings; lifecycle configuration must still be verified separately when applicable.

![S3 bucket Block Public Access settings](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-block-public-access.png)

*`Block all public access` is `On`, preventing public access through ACLs and bucket policies.*

![S3 bucket default encryption settings](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-encryption.png)

*Default server-side encryption uses Amazon S3 managed keys (SSE-S3).*

![S3 bucket versioning settings](/images/5-Workshop/5.3-Data-layer/5.3.1-provision-storage/s3-bucket-versioning.png)

*Bucket Versioning is `Enabled`; MFA delete is `Disabled`.*

## 4. Verify Logical Prefixes

Fetch at most one object per prefix to prevent unnecessary data transfers:

```bash
aws s3api list-objects-v2 \
  --bucket "<S3_BUCKET_NAME>" \
  --prefix "<RAW_PREFIX>" \
  --max-items 1
```

Repeat for `processed`, `training`, `inference`, `models`, `evaluation`, and `interaction exports` prefixes.

An empty prefix list is not necessarily an error. `AccessDenied`, incorrect region settings, or non-existent buckets indicate issues that must be resolved.

## 5. Handling Missing Infrastructure

If tables or buckets are missing:

1. Pause deployment activities.
2. Record the required region, key schemas, billing modes, TTL settings, encryption, lifecycle policies, and IAM owners.
3. Request the platform or security team to provision required resources or provide reviewed IaC templates.

{{% notice warning %}}
Do not create tables or buckets using unreviewed CLI commands. Legacy documentation in the ML submodule describes a schema that differs from the current backend schema and must not be used as an official runbook.
{{% /notice %}}

## Completion Criteria

- [ ] All five DynamoDB tables are `ACTIVE`.
- [ ] Key schemas match requirements exactly.
- [ ] The S3 bucket is accessible.
- [ ] Block Public Access and server-side encryption are configured.
- [ ] `expire_at` TTL state is recorded as verified or unconfirmed.
- [ ] No infrastructure resources were mutated during the verification step.

**Reference Sources:** `backend/app/aws/infrastructure.py`, `docs/aws/dynamodb.md`, and `docs/aws/aws-setup.md`.
