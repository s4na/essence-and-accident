# Rails の普通

Rails では、フレームワークの既存レールとアプリ内の近傍パターンを最優先する。新しい層や抽象化を足す前に、既存の model / controller / view / job / mailer / policy / form などの置き場所で足りるかを確認する。

## 基本方針

- Rails 標準の命名、配置、autoload、routing、Active Record の慣習に従う。
- 近い既存実装の形に寄せる。一般論のベストプラクティスより、このアプリの普通を優先する。
- 小さな機能追加で新しい architecture layer を作らない。
- 「将来増えそう」だけで STI、state machine、service object、concern、callback、汎用基底クラスを追加しない。
- security と data integrity は最小差分より優先する。

## Model / DB

- DB に保存するのは事実であり、可能なら状態は事実から導出する。
- `status` カラムや enum は、重複した真実や不正状態を作りやすいのでデフォルトでは避ける。
- nullable カラムを増やす場合は、nil がドメイン上の意味を持つか、移行中だけのものかを明示する。
- migration では、可能な限り DB 制約、default、index、foreign key を適切に置く。
- 既存テーブルで自然に表現できる要件のために新規テーブルを作らない。
- STI はデフォルトで避ける。型ごとの差分が増えるなら composition、別テーブル、明示的な分岐などを現要件ベースで比較する。

## Controller / Routing

- RESTful な既存 routing に寄せる。
- controller は HTTP 境界として薄く保つ。ただし、そのアプリが controller に処理を置く慣習なら局所一貫性を優先する。
- 独自 action や nested route を増やす前に、既存 resource の member / collection で表現できるか確認する。

## View / Form

- 既存の partial、helper、component、form builder の使い方に従う。
- 新しい component 抽象を作る前に、既存 partial の拡張で足りるか確認する。
- validation message、I18n、formatting は既存の置き場所に合わせる。

## Service object / Concern / Callback

- service object は、既存コードベースで普通に使われている場合だけ選択肢にする。
- concern は複数の実在する利用箇所があり、抽出後の責務名が明確な場合に限る。
- callback は副作用が見えづらくなるため、既存慣習がない限り避ける。

## テスト

- 既存の test framework、factory、fixture、system/request/model spec の分け方に従う。
- 実装都合の private method テストより、外から見える振る舞いをテストする。
- DB 制約や validation を追加したら、失敗ケースも確認する。
