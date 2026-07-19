# Backend examples

DB、モデル、controller、API、worker、server-side domain logic 周辺の例をまとめています。

対応する正規ルールは [`../../references/rails.md`](../../references/rails.md) です。

ここでは「バックエンドなら普通こう置く」「この程度なら永続化・状態・非同期・層を増やさない」という判断を、すべて Markdown 内のインラインコードで示します。サンプルコードは Rails が中心ですが、意図は Rails 固有ではなくバックエンド一般の制約です。

## 一覧

- [既存モデルに小さな振る舞いを追加する](./small-behavior.md)
- [status カラムを増やさず事実から状態を導出する](./derived-state.md)
- [既存エンドポイントに最小差分でフィールドを追加する](./api-minimal-field.md)
- [新規テーブル禁止](./no-new-table.md)
- [新規カラム・nullable カラム禁止](./no-new-column.md)
- [STI / 継承階層禁止](./no-sti.md)
- [service object / concern 禁止](./no-service-object-concern.md)
- [callback 禁止](./no-callback.md)
- [background job 禁止](./no-background-job.md)
- [feature flag 禁止](./no-feature-flag.md)
- [state machine 禁止](./no-state-machine.md)
- [新しい layer / framework convention 禁止](./no-new-layer-convention.md)
- [現在要件のみ](./current-requirements-only.md)
- [不正状態を表現可能にしない](./invalid-state.md)
- [security / correctness 非交渉](./security-correctness.md)
- [新規概念の立証責任](./burden-of-proof.md)
