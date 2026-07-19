# CI/CD 変更の最小差分: 必要な check だけ足す

## 要件

PR で Markdown のリンク切れチェックを実行したい。

## やらないこと

- CI workflow 全体を作り直さない
- build / deploy job の依存関係を変えない
- cache 戦略を全面変更しない
- 新しい release process を混ぜない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存 CI に Markdown link check step だけ追加する
- 棄却する実装: documentation 専用 pipeline の新設

## 普通の実装の全体像

```yaml
# .github/workflows/ci.yml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Check Markdown links
        run: npx markdown-link-check README.md
```

## レビュー観点

- 目的の check 以外の workflow 構造を変えていないか
- deploy / release 経路に影響していないか
- CI 時間とネットワーク依存を説明できるか
