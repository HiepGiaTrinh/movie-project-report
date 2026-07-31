---
title: "Summary and Resource Cleanup"
date: 2026-07-30
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

This workshop has covered:

- Data pipeline workflows and S3 artifact layouts.
- Five DynamoDB tables and their associated access patterns.
- Implicit ALS model training, evaluation, and promotion gate mechanisms.
- SageMaker Processing Job execution.
- FastAPI provider architecture, caching, and controlled failure paths.
- Application deployment patterns on EC2 hosts.
- IAM boundaries enforcing the principle of least privilege.

## Potential Cost-Incurring Resources

- SageMaker real-time endpoints and endpoint instances.
- Active SageMaker Processing Jobs during execution.
- EC2 instances and attached EBS storage volumes.
- DynamoDB request units, backups, and storage tables.
- S3 current objects, non-current versions, and incomplete multipart uploads.
- CloudWatch Logs log groups and retention.
- Data transfer egress bandwidth.
- Optional network resources not detailed in the repository.

## Resource Cleanup Sequence

{{< mermaid align="center" >}}
flowchart TD
    A[Stop traffic, CI deployments, and schedulers] --> B[Archive required logs, reports, and artifacts]
    B --> C[Stop endpoints, jobs, and EC2 instances]
    C --> D[Delete endpoints before EndpointConfigs and Models]
    D --> E[Export data before deleting DynamoDB tables]
    E --> F[Delete objects and non-current versions before S3 bucket]
    F --> G[Detach policies before deleting IAM roles]
    G --> H[Audit AWS Billing and resource inventory]
{{< /mermaid >}}

Execute cleanup operations in the following strict order:

1. Stop incoming traffic, CI deployment workflows, and retraining timers.
2. Export or archive logs, evaluation reports, model manifests, and required data.
3. Stop active SageMaker processing jobs.
4. Delete SageMaker Endpoints before EndpointConfigs and Models (if confirmed to belong strictly to workshop scope).
5. Stop or terminate EC2 instances according to organizational data retention policies.
6. Export required DynamoDB items before deleting test tables or items.
7. Delete S3 current objects, non-current versions, and incomplete multipart uploads before deleting buckets.
8. Detach managed/inline policies before deleting IAM roles.
9. Verify AWS Billing, AWS Cost Explorer, and resource inventory dashboards.

## Why Specific Delete Commands Are Omitted

{{% notice warning %}}
The repository contains no automated cleanup scripts or IaC configurations matching the current infrastructure architecture. Executing arbitrary deletion CLI commands risks accidentally destroying shared buckets or production tables. Consequently, this section provides read-only inventory commands; resource owners must provide approved runbooks and explicit deletion target lists.
{{% /notice %}}

## Potential Permanent Data Loss Risks

- User account credentials and password hashes.
- Historical `UserInteractions` used for retraining.
- Cached recommendations in `RecommendationCache`.
- Movie metadata catalog and popular movie rankings.
- Raw and processed dataset files.
- Model binary artifacts, manifests, and evaluation history.
- S3 object versions.
- Log files and audit trail evidence.

## Read-Only Resource Inventory Verification

The following CLI commands only list resources and perform zero deletion operations:

```bash
aws sagemaker list-endpoints \
  --region "<AWS_REGION>"

aws sagemaker list-processing-jobs \
  --region "<AWS_REGION>"

aws ec2 describe-instances \
  --region "<AWS_REGION>" \
  --filters Name=instance-state-name,Values=running

aws dynamodb list-tables \
  --region "<AWS_REGION>"

aws s3api list-objects-v2 \
  --bucket "<S3_BUCKET_NAME>" \
  --max-items 10
```

<!-- IMAGE-5.6-01: Resource inventory console output after cleanup with Account IDs and ARNs redacted. -->

{{% notice warning %}}
Do not use bucket or table names from `.env.example` as cleanup target inputs. Always verify AWS Account IDs, regions, resource ARNs, owners, retention rules, and shared-resource classifications prior to executing deletion actions.
{{% /notice %}}

## Post-Cleanup Checklist

- [ ] Zero active endpoints, EC2 instances, or Processing Jobs remain outside retained inventory lists.
- [ ] Test data and model artifacts are deleted only after receiving data retention approvals.
- [ ] S3 object versions were audited, not solely current objects.
- [ ] IAM roles are deleted only after all referencing resources are removed.
- [ ] AWS Billing and resource inventory dashboards have been verified.
- [ ] Zero cleanup CLI commands were executed automatically from report text.

**Unimplemented Gaps Requiring Resolution:** Reviewed cleanup scripts, data retention policies, backup procedures, resource ownership tags, shared-resource classification, and billing verification runbooks.
