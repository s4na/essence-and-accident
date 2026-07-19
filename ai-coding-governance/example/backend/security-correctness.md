# security / correctness 非交渉: 最小差分でも安全性は削らない

## 要件

管理画面でメールアドレスからユーザーを検索したい。

## やらないこと

- SQL 文字列へ入力値を直接埋め込まない
- 例外を握りつぶさない
- `any` で型を広げない

## decision diff

- 新しい概念: なし
- 採用する実装: パラメータ化された query / 既存 ORM API を使う

## 普通の実装の全体像

```ruby
# app/controllers/admin/users_controller.rb
class Admin::UsersController < Admin::BaseController
  def index
    @users = User.order(:id)
    @users = @users.where("email LIKE ?", "%#{User.sanitize_sql_like(params[:email])}%") if params[:email].present?
  end
end
```

```ruby
# test/controllers/admin/users_controller_test.rb
test "searches by email safely" do
  get admin_users_path(email: "alice@example.com")
  assert_response :success
end
```

## レビュー観点

- 「最小差分」を言い訳に injection を許していないか
- 入力値の扱いが既存の安全な pattern と一致しているか
