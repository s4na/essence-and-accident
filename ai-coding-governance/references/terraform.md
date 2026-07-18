# Terraform の普通

Terraform では、状態ファイルに残る永続的なインフラ差分を最小化する。module、variable、resource、provider 設定を増やす前に、既存構成で表現できるかを確認する。

## 基本方針

- 既存の module 構成、命名、tag / label、workspace / environment 分割に従う。
- 小さな変更で新規 module を作らない。
- provider version、backend、state 管理、workspace 方針を勝手に変えない。
- plan 上の destroy / replace は明示的に説明し、ユーザー承認なしに進めない。
- 手動作成済みリソースを Terraform 管理に入れる場合は import 方針を先に確認する。

## Resource / Module

- resource は具体的に命名し、既存 naming convention に合わせる。
- variable は必要な外部入力だけにする。将来のための汎用 variable を増やさない。
- output は実際に他 module や人間が必要とするものだけにする。
- locals は重複削減や命名整理には使うが、複雑な疑似プログラムにしない。
- module 化は、複数の実在利用箇所か明確な境界がある場合だけ行う。

## State / Safety

- `terraform plan` をレビュー単位にする。
- lifecycle の `ignore_changes`、`create_before_destroy`、`prevent_destroy` は意図を明示する。
- secret を state に入れない。sensitive 指定だけで安全になると誤解しない。
- drift を隠すためだけに ignore_changes を追加しない。

## Variables

- default を置く場合は環境差分とセキュリティ影響を確認する。
- nullable variable を増やすより、明示的な object 型や validation を検討する。
- `map(any)` や `object({})` で型を広げない。

## Testing / Check

- 変更後は少なくとも `terraform fmt` と `terraform validate` を実行する。
- 可能なら対象 workspace / directory で `terraform plan` を実行し、差分要約を説明する。
