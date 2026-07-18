# API: 既存エンドポイントに最小差分でフィールドを追加する

## 要件

既存のユーザー詳細 API に、ユーザーの表示名 `displayName` を追加したい。

既存 DB には `first_name` と `last_name` がある。

## やらないこと

この要件だけでは、次は追加しません。

- `display_name` カラム
- `UserProfile` テーブル
- 汎用 serializer 基底クラス
- API バージョン追加
- `DisplayNameService`
- 名前フォーマット設定
- 多言語・敬称・ミドルネーム対応

## decision diff

- 新しい概念: なし
- DB 変更: なし
- API 変更: 既存レスポンスに `displayName` を追加
- 採用する実装: 既存の `first_name` / `last_name` から表示名を導出する
- 棄却する実装: 表示名を新規カラムとして保存する

## 実装の全体像

既存フィールドから導出できる値なので、永続化せずレスポンス生成時に組み立てます。

```ts
// users/presenter.ts
type User = {
  id: string;
  firstName: string;
  lastName: string;
  email: string;
};

type UserResponse = {
  id: string;
  email: string;
  displayName: string;
};

export function presentUser(user: User): UserResponse {
  return {
    id: user.id,
    email: user.email,
    displayName: `${user.firstName} ${user.lastName}`,
  };
}
```

```ts
// users/presenter.test.ts
import { describe, expect, it } from "vitest";
import { presentUser } from "./presenter";

describe("presentUser", () => {
  it("derives displayName from firstName and lastName", () => {
    expect(
      presentUser({
        id: "user_1",
        firstName: "Ada",
        lastName: "Lovelace",
        email: "ada@example.com",
      }),
    ).toEqual({
      id: "user_1",
      email: "ada@example.com",
      displayName: "Ada Lovelace",
    });
  });
});
```

## レビュー観点

- 導出可能な値を保存していないか
- API 変更が現在要件の範囲に収まっているか
- 将来の名前仕様を予測して抽象化していないか
- 既存の presenter / serializer パターンに沿っているか
