# 新規カラム・nullable カラム禁止: 既存値から導出する

## 要件

ユーザー一覧で「名前が未設定か」を表示したい。

## やらないこと

- `name_missing` boolean カラムを作らない
- nullable な `profile_state` カラムを作らない
- バッチで導出値を同期しない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する実装: `name.blank?` から表示文言を導出する

## 普通の実装の全体像

```ruby
# app/models/user.rb
class User < ApplicationRecord
  def name_missing?
    name.blank?
  end
end
```

```erb
<!-- app/views/users/index.html.erb -->
<td><%= user.name_missing? ? "未設定" : user.name %></td>
```

## レビュー観点

- 導出可能な boolean を DB に保存していないか
- nullable カラム追加によって `nil/true/false` の三状態を作っていないか
