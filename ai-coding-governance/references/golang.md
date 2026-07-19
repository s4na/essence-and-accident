# Golang reference

- 変更前に、対象 package、呼び出し元、interface、error、context、test と近隣の Go の書き方を確認する。
- 既存 package に収まる小さな振る舞いは、まず既存の function や method として表現する。
- 一つの利用箇所のために新しい package、generic helper、抽象 interface を作らない。
- interface は実際の利用側に複数の実装差分が必要なときだけ、利用側の境界で定義する。
- error は握りつぶさず、既存の error wrapping と分類のパターンに合わせる。
- context の伝播、cancel、timeout、resource の close を既存の処理と同じ規約で扱う。
- 標準 library と既存 dependency で要件を満たせるなら、新しい dependency を追加しない。
- 現在の要件に不要な goroutine、channel、worker、retry、設定値を先回りして追加しない。
- test は既存の package 配置、fixture、table-driven test の使い方に合わせる。
