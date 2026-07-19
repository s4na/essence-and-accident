# state machine 禁止: 既存事実から分岐する

## 要件

請求書が支払い済みかどうかを表示したい。

## やらないこと

- state machine gem を入れない
- `InvoiceState` を作らない
- `pending/paid/failed/refunded` を予測して enum 化しない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する実装: `paid_at` から表示を導出する

## 普通の実装の全体像

```ruby
# app/models/invoice.rb
class Invoice < ApplicationRecord
  def paid?
    paid_at.present?
  end
end
```

```erb
<!-- app/views/invoices/show.html.erb -->
<span><%= @invoice.paid? ? "支払い済み" : "未払い" %></span>
```

## レビュー観点

- 現在の二状態表示に state machine が必要か
- `paid_at` と状態の二重管理が発生していないか
