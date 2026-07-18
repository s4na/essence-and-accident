# Rails: status カラムを増やさず事実から状態を導出する

## 要件

注文がキャンセル済みかどうかを画面に表示したい。

既存の `orders` テーブルには `canceled_at` がある。

## やらないこと

この要件だけでは、次は追加しません。

- `status` カラム
- `enum status: { active: 0, canceled: 1 }`
- state machine
- `OrderState` クラス
- キャンセル履歴テーブル

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 不変条件: `canceled_at` がある注文はキャンセル済み
- 採用する実装: 表示用の状態は `canceled_at` から導出する
- 棄却する実装: `status` を追加して `canceled_at` と同期する

## 実装の全体像

保存するのは「キャンセルされた時刻」という事実です。画面表示に必要な状態名は、その事実からメソッドで導出します。

```ruby
# app/models/order.rb
class Order < ApplicationRecord
  def canceled?
    canceled_at.present?
  end

  def display_state
    canceled? ? "キャンセル済み" : "受付中"
  end
end
```

```erb
<!-- app/views/orders/show.html.erb -->
<dl>
  <dt>注文状態</dt>
  <dd><%= @order.display_state %></dd>
</dl>
```

```ruby
# test/models/order_test.rb
require "test_helper"

class OrderTest < ActiveSupport::TestCase
  test "display_state is derived from canceled_at" do
    assert_equal "キャンセル済み", Order.new(canceled_at: Time.current).display_state
    assert_equal "受付中", Order.new(canceled_at: nil).display_state
  end
end
```

## レビュー観点

- `status = canceled` だが `canceled_at` がない、という不正状態を作っていないか
- 表示都合だけの永続化をしていないか
- 既存の事実から導出できるものを DB に保存していないか
