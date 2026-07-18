# Terraform AWS の普通

AWS では IAM、network、security group、KMS、S3 public access などの安全側デフォルトを崩さない。便利さのために広い権限や公開設定を追加しない。

## 基本方針

- 既存の account / region / environment / tag 方針に従う。
- IAM policy は least privilege を基本にする。`*` action / resource は強い根拠が必要。
- security group は必要な inbound だけ許可する。`0.0.0.0/0` は user-facing public endpoint など明示理由がある場合だけ。
- S3、RDS、EBS、SNS/SQS、CloudWatch logs などの encryption / retention / public access 方針を既存に合わせる。
- AWS managed policy を安易に足さず、既存 policy pattern を確認する。

## IAM

- role、policy、attachment の責務を分け、既存 naming に合わせる。
- cross-account access、assume role、OIDC federation は trust policy を明示的にレビューする。
- service principal や condition を広げない。
- policy document は `aws_iam_policy_document` など既存手法に合わせる。

## Network / Security Group

- VPC、subnet、route table、NAT、endpoint は既存 module を優先する。
- public subnet への配置や public IP 付与は明示承認が必要。
- ingress は port、protocol、source を最小化する。
- egress all は既存方針に合わせる。新規で広げる場合は理由を書く。

## Data / Storage

- S3 は public access block、bucket ownership、encryption、versioning、lifecycle を確認する。
- RDS / ElastiCache などは backup、maintenance window、deletion protection、subnet group、security group を確認する。
- KMS key を増やす前に既存 key / alias の利用方針を確認する。

## Operational defaults

- CloudWatch log retention を無期限にしない。既存 retention に合わせる。
- alarm、dashboard、metric filter を増やす場合は運用者と actionability を確認する。
- ECS / Lambda / Batch など compute の timeout、memory、concurrency、retry は既存基準に合わせる。
