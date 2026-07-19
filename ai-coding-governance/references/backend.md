# Backend reference

バックエンドのコード変更では、DB、モデル、controller、API、worker、server-side domain logic の変更を最小概念差分で扱う。

## 原則

- 永続化を増やす前に、既存の事実から導出できないか確認する
- 新規テーブル、新規カラム、nullable カラム、`status` / state カラム、enum は原則禁止する
- state machine、STI、継承階層、service object、concern、callback、background job、feature flag は既存パターンか明示要件がない限り追加しない
- 表示都合や API レスポンス都合だけの値を DB に保存しない
- 同期で済む処理を、負荷・遅延・リトライ要件なしに非同期化しない
- controller / model / presenter / serializer などの置き場所は、近傍の既存パターンを優先する

## decision diff で確認すること

- DB / schema 変更の有無
- 新規概念・新規責務境界の有無
- どの不変条件を守るか
- 既存事実から導出できる状態を保存していないか
- 失敗時の挙動を既存パターンから逸脱していないか

## 人間向け examples

実態のあるコード例が必要な場合だけ、[examples/backend/](./examples/backend/) を参照する。
