# 新規依存禁止: 標準 API で足りるなら使う

## 要件

日付を `YYYY-MM-DD` で表示したい。

## やらないこと

- date formatting library を追加しない
- アプリ全体の日時 abstraction を作らない
- timezone policy を今回の要件で決め直さない

## decision diff

- 新しい概念: なし
- 依存追加: なし
- 採用する実装: 既存の標準 API / helper を使う

## 普通の実装の全体像

```ts
// app/lib/formatDate.ts
export function formatDate(date: Date): string {
  return date.toISOString().slice(0, 10);
}
```

```ts
// app/lib/formatDate.test.ts
import { expect, it } from "vitest";
import { formatDate } from "./formatDate";

it("formats date as YYYY-MM-DD", () => {
  expect(formatDate(new Date("2026-07-18T12:00:00.000Z"))).toBe("2026-07-18");
});
```

## レビュー観点

- 1つの formatting のために依存を増やしていないか
- 既存 helper があるならそれを使っているか
