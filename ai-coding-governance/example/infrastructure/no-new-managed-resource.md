# 新規クラウドリソース禁止: 既存リソースの設定で済ませる

## 要件

既存アプリのメトリクス保持期間を 14 日から 30 日に延ばしたい。

## やらないこと

- 新しい監視 SaaS を追加しない
- 新しい metrics database を作らない
- dashboard / alerting stack を作り直さない
- 将来の監査要件を予測して archive bucket を作らない

## decision diff

- 新しい概念: なし
- 新規クラウドリソース: なし
- 採用する実装: 既存 monitoring resource の retention 設定だけ変更する
- 棄却する実装: 将来の分析用途を見越した別 datastore 追加

## 普通の実装の全体像

```hcl
# terraform/monitoring.tf
resource "example_monitoring_workspace" "app" {
  name                  = "app-production"
  metrics_retention_days = 30
}
```

## レビュー観点

- retention 変更だけで満たせる要件に新規 managed resource を追加していないか
- コスト・権限・バックアップ・障害対応の対象を増やしていないか
- 将来の分析基盤を今回の差分に混ぜていないか
