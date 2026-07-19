# service object / concern 禁止: 既存責務に置く

## 要件

記事タイトルの表示用 trim を統一したい。

## やらないこと

- `ArticleTitleFormatterService` を作らない
- `DisplayTitleConcern` を作らない
- 全モデル汎用の title formatter を作らない

## decision diff

- 新しい概念: なし
- 採用する実装: `Article` に表示用メソッドを置く

## 普通の実装の全体像

```ruby
# app/models/article.rb
class Article < ApplicationRecord
  def display_title
    title.to_s.strip
  end
end
```

```erb
<!-- app/views/articles/_article.html.erb -->
<h2><%= article.display_title %></h2>
```

## レビュー観点

- 1モデルの小さな整形で service / concern を増やしていないか
- 既存モデルの責務として自然か
