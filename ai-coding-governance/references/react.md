# React reference

- 変更前に、対象 component、props、state、hook、画面遷移、test と近隣の React 実装を確認する。
- component 内だけで完結する UI state は、まずその component の local state として扱う。
- state は実際に共有が必要になったときだけ親や既存の共有境界へ持ち上げる。
- props や state から導出できる値を別 state に複製せず、render 時に導出する。
- `useEffect` は外部システムとの同期に使い、単なる値の計算や event 処理のために追加しない。
- 一つの画面のために generic base component、context、store、design system abstraction を新設しない。
- loading、empty、error、success の状態は、ユーザーが理解できる表示と復旧操作へつなげる。
- component の分割は実際の責務境界や再利用が必要なときに行い、見た目の整理だけで周辺を作り替えない。
- 既存の props、hook、accessibility、render、test のパターンに合わせる。
