# local consistency 優先: このリポジトリの普通に合わせる

## 要件

既存の設定画面に説明文を 1 行追加したい。

## やらないこと

- 外部 UI ライブラリの `Description` コンポーネントを導入しない
- この画面だけ CSS-in-JS に変えない
- 既存 class naming から逸脱しない

## decision diff

- 新しい概念: なし
- 採用する実装: 近くの説明文と同じ markup / class を使う

## 普通の実装の全体像

```erb
<!-- app/views/settings/show.html.erb -->
<section class="settings-section">
  <h2>通知</h2>
  <p class="settings-description">通知の受け取り方を設定できます。</p>
</section>
```

## レビュー観点

- 一般的ベストプラクティスより既存ローカルパターンを優先しているか
- この差分だけ別の UI 規約を持ち込んでいないか
