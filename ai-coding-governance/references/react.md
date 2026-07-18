# React の普通

React では、状態を増やさず、props と既存コンポーネント構造で表現できる範囲に留める。新しい状態管理、抽象コンポーネント、hook、context はデフォルトでは作らない。

## 基本方針

- 既存の component 分割、ディレクトリ構成、CSS 手法、data fetching パターンに従う。
- まず近い既存 component を読む。
- 小さな UI 変更で共通 component、context、global state、custom hook を新設しない。
- derived state を useState に保存しない。props、server data、既存 state から計算する。
- 不要な memoization をしない。`useMemo` / `useCallback` は性能上の根拠か参照安定性の必要がある場合だけ使う。

## Component

- component は具体的な用途の名前にする。早すぎる `Base*`、`Generic*`、`Shared*` を避ける。
- props は必要最小限にし、boolean props の組み合わせで不正状態を作らない。
- children / render props / polymorphic component は、既存パターンがある場合か現要件で必要な場合だけ使う。
- layout と domain behavior を混ぜない。ただし既存の局所慣習を優先する。

## State

- state は最も狭い所有者に置く。
- URL、server state、form state、UI-only state を混ぜない。
- 同じ意味を複数 state に保存しない。
- `isLoading` / `isError` / `status` などを手で同期するより、既存 data fetching library の状態を使う。

## Effects

- `useEffect` は外部システムとの同期に使う。単なる値の導出やイベント処理を effect に逃がさない。
- dependency array を lint 回避のために改変しない。
- cleanup が必要な subscription / timer / listener は必ず cleanup する。

## Error / Accessibility

- 既存の error 表示、toast、boundary、form validation の流儀に従う。
- button、label、aria、keyboard 操作などの基本的な accessibility を落とさない。
- loading / empty / error state は、既存 UI パターンで最小限に扱う。
