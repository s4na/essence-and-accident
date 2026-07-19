# feature flag 禁止: 仕様として出すか出さないかを決める

## 要件

ユーザー詳細に電話番号を表示する。

## やらないこと

- `show_phone_number` feature flag を作らない
- flag 管理画面や環境変数を追加しない
- flag 分岐のテストを増やさない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存詳細画面に常時表示する
- 棄却する実装: 未要求の段階リリースを予測した flag

## 普通の実装の全体像

```erb
<!-- app/views/users/show.html.erb -->
<dl>
  <dt>電話番号</dt>
  <dd><%= @user.phone_number.presence || "未登録" %></dd>
</dl>
```

## レビュー観点

- リリース制御が要件にないのに flag を追加していないか
- flag が永続的な分岐として残る設計になっていないか
