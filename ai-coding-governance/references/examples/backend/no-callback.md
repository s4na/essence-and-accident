# callback 禁止: 呼び出し元で明示する

## 要件

プロフィール更新後に、画面上の表示名プレビューを更新したい。

## やらないこと

- `after_save` callback で派生値を保存しない
- hidden な副作用で別カラムを更新しない
- observer を作らない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する実装: 表示時に `display_name` を導出する

## 普通の実装の全体像

```ruby
# app/models/profile.rb
class Profile < ApplicationRecord
  def display_name
    [first_name, last_name].compact_blank.join(" ")
  end
end
```

```erb
<!-- app/views/profiles/show.html.erb -->
<p><%= @profile.display_name %></p>
```

## レビュー観点

- callback により更新経路が見えなくなっていないか
- 保存済みの派生値と元データの不整合が起きないか
