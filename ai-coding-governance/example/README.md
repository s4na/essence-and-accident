# Examples

このディレクトリは、`references/` にある箇条書きのルールを、人間が具体的な要件・コード・レビューの流れとして理解するための例です。言語やフレームワーク名ではなく、コードの責務領域ごとにまとめています。

目的は、正解の設計パターン集でも、AI が実行時に読むルール一覧でもありません。各種変更を **最小概念差分**・**既存パターン優先**・**decision diff 先行**で扱うと、人間向けにはどう見えるかを示します。

## 読み方

まずレビューしたい対象に近いディレクトリを開きます。

- DB、モデル、controller、API、worker、server-side domain logic などバックエンドのコードなら [`backend/`](./backend/)
- UI、component、client-side state、型、依存、画面差分などフロントエンドのコードなら [`frontend/`](./frontend/)
- Terraform、Kubernetes、CI/CD、環境変数、クラウドリソースなどインフラのコードなら [`infrastructure/`](./infrastructure/)
- decision diff、境界設計、承認後停止、review、commit などメタなアーキテクチャ判断なら [`architecture/`](./architecture/)

各ドキュメントは、対応する reference の箇条書きを前提に、次の順番で読みます。

1. 要件
2. やらないこと
3. decision diff
4. 普通の実装の全体像（インラインのコード例を含む）
5. レビュー観点

コード例はすべて Markdown 内にインラインで置いています。実際のアプリにそのままコピーするためではなく、AI に期待する変更の粒度・責務境界・禁止したい過剰設計を共有するためのものです。

## メンテナンス上の位置づけ

`references/` がルールの正本で、`example/` はその理解を助ける具体例です。reference のすべての箇条書きに一対一の example が必要なわけではありません。人間が判断の違いを理解するのに有効なケースだけを example として置きます。

reference を変更したときは、対応する example の説明、コード、レビュー観点が古くなっていないか確認します。example だけを変更して新しいルールを追加することはしません。

## ディレクトリ別一覧

対応する reference は次のとおりです。

- Backend / Rails: [`../references/rails.md`](../references/rails.md)
- Frontend: [`../references/frontend.md`](../references/frontend.md)
- Infrastructure: [`../references/infrastructure.md`](../references/infrastructure.md)
- Core / Architecture: [`../references/core.md`](../references/core.md)

### Backend / Rails

- [既存モデルに小さな振る舞いを追加する](./backend/small-behavior.md)
- [status カラムを増やさず事実から状態を導出する](./backend/derived-state.md)
- [既存エンドポイントに最小差分でフィールドを追加する](./backend/api-minimal-field.md)
- [新規テーブル禁止](./backend/no-new-table.md)
- [新規カラム・nullable カラム禁止](./backend/no-new-column.md)
- [STI / 継承階層禁止](./backend/no-sti.md)
- [service object / concern 禁止](./backend/no-service-object-concern.md)
- [callback 禁止](./backend/no-callback.md)
- [background job 禁止](./backend/no-background-job.md)
- [feature flag 禁止](./backend/no-feature-flag.md)
- [state machine 禁止](./backend/no-state-machine.md)
- [新しい layer / framework convention 禁止](./backend/no-new-layer-convention.md)
- [現在要件のみ](./backend/current-requirements-only.md)
- [不正状態を表現可能にしない](./backend/invalid-state.md)
- [security / correctness 非交渉](./backend/security-correctness.md)
- [新規概念の立証責任](./backend/burden-of-proof.md)

### Frontend

- [小さな UI 状態を過剰抽象化せず扱う](./frontend/small-ui-state.md)
- [generic base class / interface 禁止](./frontend/no-generic-base.md)
- [新規依存禁止](./frontend/no-new-dependency.md)
- [future-proofing 禁止](./frontend/no-future-proofing.md)
- [美的理由の周辺リファクタ禁止](./frontend/no-aesthetic-refactor.md)
- [review signal の保持](./frontend/review-signal.md)
- [同じ行を何度も触らない](./frontend/touch-existing-lines-once.md)

### Infrastructure

- [新規クラウドリソース禁止](./infrastructure/no-new-managed-resource.md)
- [環境変数・設定追加の立証責任](./infrastructure/no-unapproved-config.md)
- [CI/CD 変更の最小差分](./infrastructure/minimal-ci-change.md)

### Core / Architecture

- [decision diff と実装の分離](./architecture/decision-diff-full-flow.md)
- [local consistency 優先](./architecture/local-consistency.md)
- [承認後に逸脱しない](./architecture/post-approval-stop.md)
- [意味単位コミット](./architecture/meaning-based-commits.md)
- [境界を増やす前のチェック](./architecture/boundary-before-abstraction.md)
- [可逆性を優先するアーキテクチャ判断](./architecture/reversibility.md)
