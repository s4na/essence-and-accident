# STI / 継承階層禁止: 種別ごとの小さな分岐に留める

## 要件

通知文面で、メール通知とアプリ内通知の表示ラベルを出し分けたい。

## やらないこと

- `EmailNotification < Notification` の STI を作らない
- `NotificationType` 継承階層を作らない
- 種別別テーブルを作らない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する実装: 既存の `delivery_method` を表示ラベルへ変換する小さなメソッド

## 普通の実装の全体像

```ruby
# app/models/notification.rb
class Notification < ApplicationRecord
  def delivery_label
    case delivery_method
    when "email" then "メール"
    when "in_app" then "アプリ内"
    else "不明"
    end
  end
end
```

```erb
<!-- app/views/notifications/_notification.html.erb -->
<span><%= notification.delivery_label %></span>
```

## レビュー観点

- 表示ラベルの差だけで型階層を作っていないか
- 将来の通知種別を予測して抽象化していないか
