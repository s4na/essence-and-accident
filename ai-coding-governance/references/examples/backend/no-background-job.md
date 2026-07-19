# background job 禁止: 同期で十分な処理に留める

## 要件

問い合わせ送信後に確認メールを 1 通送る。

## やらないこと

- job queue を新設しない
- retry / dead letter / job status を作らない
- 非同期化に伴う状態カラムを追加しない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存 mailer の同期送信パターンに合わせる
- 承認が必要な判断: 高トラフィックや遅延要件が出た場合のみ job 化を検討

## 普通の実装の全体像

```ruby
# app/controllers/inquiries_controller.rb
class InquiriesController < ApplicationController
  def create
    @inquiry = Inquiry.create!(inquiry_params)
    InquiryMailer.received(@inquiry).deliver_now
    redirect_to thanks_inquiries_path
  end
end
```

```ruby
# test/controllers/inquiries_controller_test.rb
test "sends receipt mail" do
  assert_emails 1 do
    post inquiries_path, params: { inquiry: { email: "a@example.com", body: "hello" } }
  end
end
```

## レビュー観点

- 現在の負荷要件なしに非同期基盤を増やしていないか
- job status などの管理対象を増やしていないか
