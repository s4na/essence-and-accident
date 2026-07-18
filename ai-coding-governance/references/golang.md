# Go の普通

Go では、小さく明示的な構造、標準ライブラリ優先、単純な error handling、interface の受け手側定義を基本にする。抽象化より読みやすい直線的なコードを優先する。

## 基本方針

- 既存 package 構成、命名、error handling、logging、context の渡し方に従う。
- 小さな機能追加で framework、generic abstraction、interface、package を増やさない。
- 標準ライブラリで足りるなら新依存を追加しない。
- magic より明示を優先する。
- concurrency は現要件で必要な場合だけ使う。

## Package / Naming

- package 名は短く具体的にする。`common`、`utils`、`shared` は既存にない限り避ける。
- exported name は外部利用がある場合だけにする。
- ファイル分割は責務境界を明確にするために行い、型ごとに過剰分割しない。
- circular dependency を避けるための巨大 interface や package 抽出を安易にしない。

## Interface

- interface は利用側で小さく定義する。
- 実装が一つしかなく、テストや境界に必要でない interface は作らない。
- `Manager`、`Service`、`Provider` など曖昧な名前を避ける。
- mock のためだけに大きな interface を作らない。既存テスト方針を確認する。

## Error / Context

- error は握りつぶさない。必要なら `%w` で wrap する。
- sentinel error、custom error type、status mapping は既存方針に合わせる。
- `context.Context` は request 境界から明示的に渡す。struct に保存しない。
- panic は初期化失敗など既存方針で許される場合に限る。

## Concurrency

- goroutine、channel、mutex は必要性を説明できる場合だけ使う。
- goroutine leak、context cancellation、error propagation を確認する。
- 並列化で順序や retry semantics が変わる場合は decision diff に出す。

## Tests

- table-driven tests は既存方針に合わせて使う。
- 外部境界は小さな fake / stub を使い、過剰な mock framework を足さない。
- race が関係する変更では `go test -race` を検討する。
