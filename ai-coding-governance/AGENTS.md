# AGENTS.md

このディレクトリ配下の `ai-coding-governance` スキルを変更するときは、次のルールに従うこと。

## Reference の正本ルール

`references/` は、AI が作業時に参照する正規の運用ルールとして扱う。`SKILL.md` は実行手順、`references/` は通常の実装方針と制約を担当する。

- 新しい判断基準や通常の実装方針は、最初に該当する `references/*.md` に追加すること
- reference の本文は、AI が判断に使える短い箇条書きを基本とすること
- 同じルールを `SKILL.md` に重複して書かず、必要なら reference へのリンクだけを追加すること
- Rails、frontend、infrastructure のいずれにも属さない作業は `references/core.md` に置くこと
- reference 間で矛盾するルールを作らず、例外が必要なら decision diff で承認を求めること

## Examples の役割

`example/` は、reference の箇条書きを人間が具体的なケースとして理解・レビューするための解説資料として扱う。スキル実行時の正本や、ルールの網羅リストにはしないこと。

- reference の変更時は、影響を受ける example があるかを確認すること
- example を追加・更新する場合は、対応する reference の項目を `example/README.md` の対応表に反映すること
- example は reference の文章を再掲するだけにせず、要件、やらないこと、decision diff、普通の実装、レビュー観点を説明すること
- 具体例が不要な抽象的な手順変更では、example を無理に追加しないこと

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

`example/README.md` は、reference の項目と人間向け example ドキュメントの対応表として機能する。

項目を追加・移動・削除した場合は、リンク切れや対応漏れが残らないように `example/README.md` と各カテゴリの `README.md` を更新すること。

## スキル本文との整合性

`README.md`、`SKILL.md`、`references/`、`example/`、`agents/openai.yaml` の説明が矛盾しないようにすること。

特に、スキル本文で「禁止」「要承認」「停止」としている項目について、example 側でそれを通常実装として許可するような書き方をしないこと。
