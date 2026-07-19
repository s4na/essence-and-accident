# Infrastructure reference

インフラのコード変更では、Terraform、Kubernetes、CI/CD、環境変数、クラウドリソースを、運用対象を増やさない方向で扱う。

## 原則

- 新規 managed resource は、コスト・権限・監視・バックアップ・障害対応の恒久コストを増やすため原則禁止する
- 環境変数や設定項目は、値の分岐を運用・テスト対象として増やすため原則禁止する
- CI/CD の変更は、目的の check / step だけに留め、build / deploy / release 経路を不用意に変えない
- 既存 resource の設定変更で満たせる要件に、新規 stack や SaaS を追加しない
- 将来の監査・分析・負荷要件を予測してリソースを増やさない

## decision diff で確認すること

- 新規 resource、role、secret、env var、workflow、job の有無
- コスト・権限・監視・障害対応の対象が増えるか
- deploy / rollback / incident response に影響するか
- 既存 resource の設定変更で済まない理由があるか

## 人間向け examples

実態のあるコード例が必要な場合だけ、[examples/infrastructure/](./examples/infrastructure/) を参照する。
