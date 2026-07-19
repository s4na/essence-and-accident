# 意味単位コミット: ファイル単位ではなく判断単位で分ける

## 要件

ユーザー一覧に最終ログイン列を追加し、テストする。

## 普通のコミット構成

```text
commit 1: Add last sign-in column to admin users table
- 既存 view に列を追加
- controller や schema は変更しない

commit 2: Cover last sign-in display in system test
- 表示確認だけを追加
```

## 避けるコミット構成

```text
commit 1: Refactor admin user table
commit 2: Add presenter
commit 3: Add last sign-in
commit 4: Remove presenter
commit 5: Fix formatting
```

## レビュー観点

- 各コミットが人間の判断単位になっているか
- 同じコードを複数回レビューさせる構成になっていないか
