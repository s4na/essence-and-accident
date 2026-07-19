# Decision diff と実装の分離

## 要件

管理画面のユーザー詳細に「最終ログイン日時」を表示したい。

## やらないこと

- ログイン履歴テーブルを作らない
- ユーザー状態管理を作らない
- serializer 層を新設しない
- 未要求の「ログイン回数」「端末別履歴」「監査ログ」を作らない

## decision diff

- 新しい概念: なし
- DB 変更: なし。既存の `last_sign_in_at` を使う
- 変更ファイル: 既存の詳細 view と既存テストだけ
- 既存パターン: 管理画面の他フィールドと同じ `<dl>` 表示
- 承認が必要な判断: なし

## 普通の実装の全体像

```erb
<!-- app/views/admin/users/show.html.erb -->
<dl>
  <dt>メールアドレス</dt>
  <dd><%= @user.email %></dd>

  <dt>最終ログイン</dt>
  <dd><%= l(@user.last_sign_in_at) if @user.last_sign_in_at.present? %></dd>
</dl>
```

```ruby
# test/system/admin/users_test.rb
test "admin can see last sign in time" do
  user = users(:alice)
  visit admin_user_path(user)
  assert_text I18n.l(user.last_sign_in_at)
end
```

## レビュー観点

- decision diff にない設計判断が実装で増えていないか
- 既存 field 表示と同じ粒度か
- 監査ログなどの未来要件を混ぜていないか
