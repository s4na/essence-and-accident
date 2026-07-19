# AGENTS.md

このディレクトリ配下の `ai-coding-governance` スキルを変更するときは、次のルールに従うこと。

## Skill / references / examples の役割

このスキルは、次の三層構造で保守する。

1. `SKILL.md`: スキルの入口。実行手順と、どの reference を読むかだけを短く示す。
2. `references/*.md`: スキル実行時に参照される実務ルール。AI は通常ここまでを読む。
3. `references/examples/`: 人間が reference を理解するための補助資料。通常のスキル実行では読ませない。

`references/examples/` を `SKILL.md` から直接参照しないこと。examples は reference から辿れる人間向けの具体例として扱う。

## References の網羅ルール

`references/` は、`SKILL.md` に書かれた制約・原則・運用プロトコルを責務領域ごとに整理する場所として扱う。

`SKILL.md` で次のいずれかを追加・削除・変更した場合は、必ず `references/*.md` も同時に更新すること。

- hard default constraints
- design principles
- pre-implementation protocol
- implementation protocol after approval
- review protocol
- commit guidance
- prompt template

## Examples の網羅ルール

`references/examples/` は、単なるサンプル集ではなく、`references/*.md` に書かれたルールが実態のあるコードになるとどう見えるかを項目ごとに示すためのディレクトリとして扱う。

`references/*.md` の実務ルールを追加・削除・変更した場合は、必要に応じて `references/examples/` も更新すること。

## Examples の配置ルール

例はフラットに増やさず、言語名・フレームワーク名ではなく、コードの責務領域ごとにまとめること。

- DB、モデル、controller、API、worker、server-side domain logic などバックエンドのコード例は `references/examples/backend/` に置く
- UI、component、client-side state、型、dependency、画面差分などフロントエンドのコード例は `references/examples/frontend/` に置く
- Terraform、Kubernetes、CI/CD、環境変数、クラウドリソースなどインフラのコード例は `references/examples/infrastructure/` に置く
- decision diff、境界設計、承認後停止、review、commit などメタなアーキテクチャ判断は `references/examples/architecture/` に置く

サンプルコードが Rails、TypeScript、Go など特定技術で書かれていても、分類は技術名ではなく責務領域で決めること。

新しい責務領域が必要な場合は、`references/README.md` と `references/examples/README.md` にカテゴリの読み方を追加し、そのカテゴリ内にも `README.md` を置くこと。

## 各 example ドキュメントの必須構成

各 example Markdown は、原則として次の見出しを持つこと。

1. 要件
2. やらないこと
3. decision diff
4. 普通の実装の全体像（インラインのコード例を含む）
5. レビュー観点

コード例は別ファイルとして runnable code を追加するのではなく、Markdown 内の fenced code block としてインラインに書くこと。

## Coverage table の更新

`references/examples/README.md` は、reference 上の項目と example ドキュメントの対応表として機能する。

項目を追加・移動・削除した場合は、リンク切れや未対応項目が残らないように `references/README.md`、`references/examples/README.md`、各カテゴリの `README.md` を更新すること。

## スキル本文との整合性

`README.md`、`SKILL.md`、`references/`、`agents/openai.yaml` の説明が矛盾しないようにすること。

特に、スキル本文や reference で「禁止」「要承認」「停止」としている項目について、example 側でそれを通常実装として許可するような書き方をしないこと。
