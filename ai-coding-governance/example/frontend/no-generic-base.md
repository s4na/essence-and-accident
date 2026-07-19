# generic base class / interface 禁止: 具体的な関数に留める

## 要件

金額を日本円表示に整形したい。

## やらないこと

- `Formatter<T>` interface を作らない
- `BasePresenter` を作らない
- 通貨抽象化を作らない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存 helper に具体的な関数を追加する

## 普通の実装の全体像

```ts
// app/lib/formatMoney.ts
export function formatYen(amount: number): string {
  return new Intl.NumberFormat("ja-JP", {
    style: "currency",
    currency: "JPY",
    maximumFractionDigits: 0,
  }).format(amount);
}
```

```ts
// app/lib/formatMoney.test.ts
import { expect, it } from "vitest";
import { formatYen } from "./formatMoney";

it("formats yen", () => {
  expect(formatYen(1200)).toBe("￥1,200");
});
```

## レビュー観点

- 1用途のために汎用 interface を作っていないか
- 具体的な名前で要件が表現されているか
