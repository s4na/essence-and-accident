# 美的理由の周辺リファクタ禁止: 目的の行だけ触る

## 要件

ボタンの文言を「保存」から「変更を保存」に変えたい。

## やらないこと

- コンポーネント分割をしない
- CSS class 命名を変えない
- 周辺 markup を整形し直さない
- i18n 化を今回始めない

## decision diff

- 新しい概念: なし
- 採用する実装: 対象文言だけ変更する

## 普通の実装の全体像

```tsx
// app/settings/SettingsForm.tsx
export function SettingsForm() {
  return (
    <form>
      {/* 既存の入力群 */}
      <button type="submit">変更を保存</button>
    </form>
  );
}
```

## レビュー観点

- diff が文言変更以外に広がっていないか
- 「ついで」の整形でレビュー範囲を増やしていないか
