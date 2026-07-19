# 意味単位コミット: ファイル単位ではなく判断単位で分ける

## 要件

ユーザー一覧に最終ログイン列を追加し、テストする。

## やらないこと

- ファイル単位で機械的にコミットを分けない
- refactor、実装、修正で同じ行を何度も触らない
- formatter だけの差分を振る舞い変更に混ぜない

## decision diff

- 新しい概念: なし
- DB 変更: なし
- 採用する単位: 表示変更とテストを、人間が判断しやすい意味単位で分ける
- 棄却する単位: ファイル種別や AI の作業手順をそのままコミット単位にする

## 普通の実装の全体像

```text
commit 1: Add last sign-in column to admin users table
- 既存 view に列を追加
- controller や schema は変更しない

commit 2: Cover last sign-in display in system test
- 表示確認だけを追加
```

避ける構成:

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
- AI の作業履歴ではなく、レビューしやすい意味単位になっているか
