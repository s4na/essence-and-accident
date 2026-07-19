# Rails: 既存モデルに小さな振る舞いを追加する

## 要件

ユーザーが公開済み記事だけを閲覧できるように、記事一覧で公開済みの記事だけを返したい。

## やらないこと

この要件だけでは、次は追加しません。

- `ArticleQuery` や `ArticleSearchService`
- `Publishable` concern
- `Article::Status` クラス
- 新しいテーブル
- 新しい `status` カラム
- 将来の予約公開・審査フロー・公開取り消し状態

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 不変条件の変更: なし
- 採用する実装: 既存の `published_at` から公開済みかどうかを判定する
- 棄却する実装: `status` カラムを追加して `draft/published` を保存する

## 実装の全体像

公開済みかどうかは `published_at` が存在するという事実から導出します。状態を二重管理しないため、`status` は保存しません。

```ruby
# app/models/article.rb
class Article < ApplicationRecord
  scope :published, -> { where.not(published_at: nil) }

  def published?
    published_at.present?
  end
end
```

```ruby
# app/controllers/articles_controller.rb
class ArticlesController < ApplicationController
  def index
    @articles = Article.published.order(published_at: :desc)
  end
end
```

```ruby
# test/models/article_test.rb
require "test_helper"

class ArticleTest < ActiveSupport::TestCase
  test "published? is derived from published_at" do
    assert Article.new(published_at: Time.current).published?
    assert_not Article.new(published_at: nil).published?
  end
end
```

## レビュー観点

- `status` と `published_at` の二重管理が発生していないか
- scope が既存モデルに収まる程度の責務か
- 将来の公開ワークフローを予測していないか
- 既存の controller の取得パターンと同じ書き方か
