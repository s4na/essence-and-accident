# 環境変数・設定追加の立証責任: 分岐を増やさない

## 要件

API のタイムアウトを 5 秒から 10 秒に変更したい。

## やらないこと

- `API_TIMEOUT_SECONDS` 環境変数を新設しない
- 環境ごとの設定分岐を増やさない
- 設定 loader / validation layer を作らない

## decision diff

- 新しい概念: なし
- 新規設定: なし
- 採用する実装: 既存の定数または既存 config の値だけ変更する
- 承認が必要な判断: 環境ごとに変える要件が明示された場合だけ config 化する

## 普通の実装の全体像

```ts
// app/lib/apiClient.ts
const REQUEST_TIMEOUT_MS = 10_000;

export async function fetchJson(url: string): Promise<unknown> {
  const controller = new AbortController();
  const timeout = setTimeout(() => controller.abort(), REQUEST_TIMEOUT_MS);

  try {
    const response = await fetch(url, { signal: controller.signal });
    return response.json();
  } finally {
    clearTimeout(timeout);
  }
}
```

## レビュー観点

- 「あとで変えるかも」で環境変数を増やしていないか
- 設定値の分岐が運用・テスト対象を増やしていないか
- 現在要件が固定値変更だけで満たせるか
