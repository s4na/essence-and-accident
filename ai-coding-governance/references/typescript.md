# TypeScript の普通

TypeScript では、型を広げて問題を隠さず、ドメイン上の不正状態を表現できないようにする。型定義は実装のための飾りではなく、境界と不変条件を表す。

## 基本方針

- `any`、過剰な type assertion、広すぎる `string` / `Record<string, unknown>` を避ける。
- 既存の型定義、schema validation、API client、domain type の置き場所に従う。
- 小さな差分で generic utility type、global type、namespace、ambient declaration を追加しない。
- runtime で不明な外部入力は、型注釈ではなく parse / validation で狭める。

## 型設計

- union は、状態の組み合わせを安全に表す場合に使う。
- optional property は「存在しないこと」が意味を持つ場合だけ使う。
- `null` と `undefined` を混在させない。既存方針に合わせる。
- boolean flags の組み合わせより、必要なら discriminated union を使う。
- ただし、現要件で不要な巨大 union や状態機械は作らない。

## 関数 / API

- public な関数の入出力型は明示する。
- inference が十分に明確な局所変数に冗長な型注釈を足さない。
- 型エラーを assertion で黙らせる前に、データ境界や型定義が間違っていないか確認する。
- error は既存の Result / throw / nullable / union などの扱いに合わせる。

## React と組み合わせる場合

- component props は必要最小限にする。
- `React.FC` を使うかどうかは既存コードに合わせる。
- event type は具体的にし、`any` で逃がさない。
- form 値は文字列入力と domain value の境界を明確にする。

## テスト / 保守

- 型だけで守る部分と runtime validation が必要な部分を分ける。
- type test を追加する場合は、既存で採用されている仕組みに従う。
- 型のためだけの大規模リファクタリングは、ユーザーの明示指示がない限り行わない。
