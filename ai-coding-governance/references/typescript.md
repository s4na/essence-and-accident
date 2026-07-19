# TypeScript reference

- 変更前に、対象 module、型、境界、compiler、lint、test と近隣の TypeScript の書き方を確認する。
- 値の意味が既存の型で表現できるなら、新しい型や utility type を増やさず既存の型を使う。
- `any` や広すぎる `unknown` で不正な値を隠さず、外部入力や API 境界で検証する。
- 実際に複数の variant がある場合だけ union や discriminant を導入し、将来の variant を先回りして作らない。
- 一つの利用箇所のために generic helper、base type、interface を新設しない。
- 標準 API と既存 dependency で要件を満たせるなら、新しい dependency を追加しない。
- 既存の module、export、error、async、test のパターンに合わせる。
- 仕様上の型変更と、機械的な型注釈・配線を分けてレビューできるようにする。
