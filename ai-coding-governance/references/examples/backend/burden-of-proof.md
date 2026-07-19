# 新規概念の立証責任: 導入前に証明する

## 要件

注文完了メールに合計金額を表示したい。

## やらないこと

この要件だけでは、次は追加しません。

## AI が提案しがちな過剰案

- `OrderTotalPresenter` を作る
- `Money` value object を導入する
- 税計算 policy を抽象化する

## decision diff

- 新しい概念: なし
- 理由: 既存の `order.total_amount` が現在要件を満たす
- 棄却: 金額抽象化は複数通貨・税仕様が出た時点で検討する

## 普通の実装の全体像

```erb
<!-- app/views/order_mailer/completed.html.erb -->
<p>合計金額: <%= number_to_currency(@order.total_amount, unit: "円", precision: 0) %></p>
```

```ruby
# test/mailers/order_mailer_test.rb
test "completed mail includes total amount" do
  mail = OrderMailer.completed(orders(:paid))
  assert_match "合計金額", mail.body.encoded
end
```

## レビュー観点

- 新規概念が「現在要件に必要」と証明されているか
- 既存 helper で十分ではないか
