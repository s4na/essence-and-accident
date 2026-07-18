# TypeScript: 小さな UI 状態を過剰抽象化せず扱う

## 要件

設定画面に「詳細設定を表示する」トグルを追加したい。

## やらないこと

この要件だけでは、次は追加しません。

- グローバル state store
- `SettingsVisibilityService`
- 汎用的な `ToggleController`
- context provider
- URL query parameter との同期
- localStorage 永続化
- 将来の複数パネル対応

## decision diff

- 新しい概念: なし
- 永続化: なし
- 採用する実装: コンポーネントローカルの boolean state
- 棄却する実装: 将来の拡張を見越した汎用 toggle 管理

## 実装の全体像

UI の一時的な開閉状態は、その画面の中だけで閉じます。型も必要以上に広げません。

```tsx
// app/settings/SettingsPage.tsx
import { useState } from "react";

export function SettingsPage() {
  const [showAdvanced, setShowAdvanced] = useState(false);

  return (
    <section>
      <h1>設定</h1>

      <button
        type="button"
        aria-expanded={showAdvanced}
        onClick={() => setShowAdvanced((current) => !current)}
      >
        詳細設定を{showAdvanced ? "隠す" : "表示する"}
      </button>

      {showAdvanced && (
        <section aria-label="詳細設定">
          <label>
            <span>通知頻度</span>
            <select name="notificationFrequency" defaultValue="daily">
              <option value="daily">毎日</option>
              <option value="weekly">毎週</option>
            </select>
          </label>
        </section>
      )}
    </section>
  );
}
```

## レビュー観点

- ローカルで済む状態をアプリ全体へ広げていないか
- `string` や union を作る必要がない boolean を複雑化していないか
- 永続化や URL 同期など、未要求の仕様を足していないか
- 既存コンポーネントの書き方と同じ粒度か
