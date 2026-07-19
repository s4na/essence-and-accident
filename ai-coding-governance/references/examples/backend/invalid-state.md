# 不正状態を表現可能にしない: 事実を保存し状態を導出する

## 要件

招待が承諾済みかどうかを表示したい。

## やらないこと

- `status = accepted` を追加しない
- `accepted_at` と `status` を同期しない
- `accepted_by_id` なしの accepted 状態を許さない

## decision diff

- 新しい概念: なし
- 採用する実装: `accepted_at` と `accepted_by` の事実から判定する

## 普通の実装の全体像

```ruby
# app/models/invitation.rb
class Invitation < ApplicationRecord
  belongs_to :accepted_by, class_name: "User", optional: true

  def accepted?
    accepted_at.present? && accepted_by.present?
  end
end
```

```erb
<!-- app/views/invitations/_invitation.html.erb -->
<%= invitation.accepted? ? "承諾済み" : "未承諾" %>
```

## レビュー観点

- `status` と事実カラムの不整合が起きないか
- `accepted_at` だけ、または `accepted_by` だけの片欠けをどう扱うか明示されているか
