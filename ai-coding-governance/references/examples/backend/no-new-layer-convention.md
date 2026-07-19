# 新しい layer / framework convention 禁止: 既存の置き場所に置く

## 要件

管理画面の一覧にユーザー数を表示したい。

## やらないこと

- `app/queries` ディレクトリを新設しない
- repository pattern を導入しない
- controller から突然 GraphQL 風 resolver を呼ばない

## decision diff

- 新しい layer: なし
- 採用する実装: 既存 controller と view のパターンに置く

## 普通の実装の全体像

```ruby
# app/controllers/admin/dashboard_controller.rb
class Admin::DashboardController < Admin::BaseController
  def show
    @user_count = User.count
  end
end
```

```erb
<!-- app/views/admin/dashboard/show.html.erb -->
<p>ユーザー数: <%= @user_count %></p>
```

## レビュー観点

- このリポジトリにない layer を今回だけ増やしていないか
- 既存画面と同じ責務境界か
