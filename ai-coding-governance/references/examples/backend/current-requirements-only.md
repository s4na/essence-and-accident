# 現在要件のみ: 予測した仕様を混ぜない

## 要件

退会済みユーザーに「退会済み」ラベルを表示したい。

## やらないこと

- 一時停止・凍結・審査中・削除予約を追加しない
- ユーザー lifecycle enum を作らない
- 管理者による状態遷移 UI を作らない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存の `deleted_at` から退会済みを導出する

## 普通の実装の全体像

```ruby
# app/models/user.rb
class User < ApplicationRecord
  def withdrawn?
    deleted_at.present?
  end
end
```

```erb
<!-- app/views/users/_user.html.erb -->
<%= tag.span("退会済み") if user.withdrawn? %>
```

## レビュー観点

- 現在要件にない lifecycle を増やしていないか
- 未来の状態を予測して DB 変更していないか
