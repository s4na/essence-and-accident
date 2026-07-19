# Frontend reference

フロントエンドのコード変更では、UI、component、client-side state、型、依存、画面差分を局所的に扱う。

## 原則

- 画面内で閉じる状態は component local state に留める
- global store、context provider、URL 同期、localStorage 永続化は明示要件なしに追加しない
- 1用途のために generic base class、汎用 interface、controller、adapter を作らない
- 型は現在要件を正確に表す最小の形にする。`any` や広すぎる union で曖昧にしない
- UI 文言や列追加だけの変更で、周辺 component の全面整形や design system 導入をしない
- 新規依存は、既存 helper / platform API / 小さな関数で満たせない場合だけ検討する

## decision diff で確認すること

- 状態のスコープが画面内か、アプリ全体か
- 永続化・URL 同期・global 化が現在要件か
- 型や抽象化が 1用途を超えて必要か
- review signal を薄める formatting / refactor が混ざっていないか

## 人間向け examples

実態のあるコード例が必要な場合だけ、[examples/frontend/](./examples/frontend/) を参照する。
