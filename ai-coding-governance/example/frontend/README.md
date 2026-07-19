# Frontend examples

UI、component、client-side state、型、依存、画面差分に関する例をまとめています。

対応する正規ルールは [`../../references/frontend.md`](../../references/frontend.md) です。

ここでは「フロントエンドなら普通こう閉じる」「UI の一時状態や表示都合を過剰に抽象化・永続化しない」という判断を示します。サンプルコードは TypeScript / React が中心ですが、意図はフロントエンド一般の制約です。

## 一覧

- [小さな UI 状態を過剰抽象化せず扱う](./small-ui-state.md)
- [generic base class / interface 禁止](./no-generic-base.md)
- [新規依存禁止](./no-new-dependency.md)
- [future-proofing 禁止](./no-future-proofing.md)
- [美的理由の周辺リファクタ禁止](./no-aesthetic-refactor.md)
- [review signal の保持](./review-signal.md)
- [同じ行を何度も触らない](./touch-existing-lines-once.md)
