# Terraform GCP の普通

GCP では project、service account、IAM、network、API enablement の境界を崩さない。広い role や project-wide 権限を安易に追加しない。

## 基本方針

- 既存の project / folder / organization / region / label 方針に従う。
- IAM は least privilege を基本にする。`roles/editor`、`roles/owner`、広い project-level binding は原則避ける。
- service account は用途ごとに具体名にし、key 発行は避ける。
- API enablement は必要な service だけに限定し、既存 module に合わせる。
- VPC、subnet、firewall、private access は既存 network 方針を優先する。

## IAM

- `google_project_iam_binding` は同じ role の member 全体を管理するため、既存管理方式と競合しやすい。既存が member 方式なら `google_project_iam_member` に合わせる。
- conditional IAM を使う場合は条件式の意図を明示する。
- service account impersonation は対象 principal と scope を最小化する。
- workload identity / OIDC 連携は trust boundary をレビューする。

## Network / Firewall

- firewall rule は direction、priority、target、source range、allowed ports を最小化する。
- `0.0.0.0/0` は public endpoint など明示理由がある場合だけ。
- private service access、Cloud NAT、VPC connector は既存 module を優先する。
- subnet range や secondary range の変更は影響が大きいため事前承認を取る。

## Data / Storage

- GCS は public access、uniform bucket-level access、versioning、retention、encryption 方針を確認する。
- Cloud SQL は backup、deletion protection、private IP、authorized networks、maintenance window を確認する。
- Secret Manager を使う場合でも、secret value を Terraform state に入れない方針を優先する。

## Operational defaults

- Cloud Logging retention、alert policy、notification channel は既存運用に合わせる。
- Cloud Run / Functions / GKE の ingress、egress、service account、concurrency、timeout は最小権限・既存方針に合わせる。
- API を増やすだけでも project の権限面が変わるため、plan で説明する。
