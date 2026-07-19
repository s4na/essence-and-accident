# Infrastructure reference

- 変更前に、対象の workflow、Terraform / Kubernetes 定義、設定、環境変数と近隣の構成を確認する。
- 既存の resource、job、check、設定の変更で要件を満たせるなら、新しい managed resource を追加しない。
- CI/CD は要件に必要な check や step だけを既存 workflow に追加する。
- 環境変数、secret、configuration key は、現在の要件に必要な理由と未設定時の挙動を示してから追加する。
- 将来環境、未要求の provider、冗長な retry、一般化した module を先回りして追加しない。
- 既存の命名、権限、secret 管理、デプロイ手順に合わせ、最小差分で変更する。
- インフラ変更でも、権限の広げ過ぎ、secret の露出、破壊的な default 変更を最小差分の名目で許容しない。
