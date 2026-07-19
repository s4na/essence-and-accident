# AGENTS.md

このディレクトリ配下の `ai-coding-governance` スキルを変更するときは、次のルールに従うこと。

## Examples の網羅ルール

`example/` は、単なるサンプル集ではなく、`SKILL.md` に書かれた制約・原則・運用プロトコルを項目ごとに網羅するためのディレクトリとして扱う。

`SKILL.md` で次のいずれかを追加・削除・変更した場合は、必ず `example/` も同時に更新すること。

- hard default constraints
- design principles
- pre-implementation protocol
- implementation protocol after approval
- review protocol
- commit guidance
- prompt template

## Examples の配置ルール

例はフラットに増やさず、言語名・フレームワーク名ではなく、コードの責務領域ごとにまとめること。

- DB、モデル、controller、API、worker、server-side domain logic などバックエンドのコード例は `example/backend/` に置く
- UI、component、client-side state、型、dependency、画面差分などフロントエンドのコード例は `example/frontend/` に置く
- Terraform、Kubernetes、CI/CD、環境変数、クラウドリソースなどインフラのコード例は `example/infrastructure/` に置く
- decision diff、境界設計、承認後停止、review、commit などメタなアーキテクチャ判断は `example/architecture/` に置く

サンプルコードが Rails、TypeScript、Go など特定技術で書かれていても、分類は技術名ではなく責務領域で決めること。

新しい責務領域が必要な場合は、`example/README.md` にカテゴリの読み方を追加し、そのカテゴリ内にも `README.md` を置くこと。

## 各 example ドキュメントの必須構成

各 example Markdown は、原則として次の見出しを持つこと。

1. 要件
2. やらないこと
3. decision diff
4. 普通の実装の全体像（インラインのコード例を含む）
5. レビュー観点

コード例は別ファイルとして runnable code を追加するのではなく、Markdown 内の fenced code block としてインラインに書くこと。

## Coverage table の更新

`example/README.md` は、スキル上の項目と example ドキュメントの対応表として機能する。

項目を追加・移動・削除した場合は、リンク切れや未対応項目が残らないように `example/README.md` と各カテゴリの `README.md` を更新すること。

## スキル本文との整合性

`README.md`、`SKILL.md`、`example/`、`agents/openai.yaml` の説明が矛盾しないようにすること。

特に、スキル本文で「禁止」「要承認」「停止」としている項目について、example 側でそれを通常実装として許可するような書き方をしないこと。
