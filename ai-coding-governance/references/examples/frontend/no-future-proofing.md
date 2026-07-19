# future-proofing 禁止: 今ある要件だけを実装する

## 要件

プロフィール画像を 1 枚表示したい。

## やらないこと

- 複数画像対応を作らない
- 並び順やメイン画像フラグを作らない
- 画像変換 pipeline を作らない
- 将来の動画対応を想定しない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存の `avatar_url` を表示する

## 普通の実装の全体像

```tsx
// app/users/UserAvatar.tsx
type UserAvatarProps = {
  avatarUrl: string | null;
  name: string;
};

export function UserAvatar({ avatarUrl, name }: UserAvatarProps) {
  if (!avatarUrl) return <span aria-label={`${name}のプロフィール画像なし`}>未設定</span>;
  return <img src={avatarUrl} alt={`${name}のプロフィール画像`} />;
}
```

## レビュー観点

- 複数画像や動画など未要求の抽象化が入っていないか
- 可逆的に後から拡張できる小さい実装か
