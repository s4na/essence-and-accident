# 新規テーブル禁止: 既存関連から表示する

## 要件

商品詳細に、その商品が属するカテゴリ名を表示したい。

## やらないこと

- `product_category_summaries` テーブルを作らない
- 表示用キャッシュテーブルを作らない
- カテゴリ履歴を作らない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する実装: 既存の `product.category.name` を表示する
- 棄却する実装: 表示高速化を予測して summary テーブルを作る

## 普通の実装の全体像

```ruby
# app/models/product.rb
class Product < ApplicationRecord
  belongs_to :category
end
```

```erb
<!-- app/views/products/show.html.erb -->
<p>
  <strong>カテゴリ</strong>
  <%= @product.category.name %>
</p>
```

```ruby
# test/system/products_test.rb
test "shows category name" do
  product = products(:desk)
  visit product_path(product)
  assert_text product.category.name
end
```

## レビュー観点

- 「表示したい」だけで集計・summary・履歴テーブルを増やしていないか
- 既存関連で到達できる事実を複製していないか
