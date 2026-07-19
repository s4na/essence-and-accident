# review signal の保持: 判断行と機械行を混ぜない

## 要件

一覧に「作成日」列を追加したい。

## やらないこと

- 同じ PR で table component を全面整形しない
- column 定義 DSL を新設しない
- unrelated な import 並び替えをしない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存 table に 1 列追加する

## 普通の実装の全体像

```tsx
// app/users/UserTable.tsx
export function UserTable({ users }: { users: User[] }) {
  return (
    <table>
      <thead>
        <tr>
          <th>名前</th>
          <th>メール</th>
          <th>作成日</th>
        </tr>
      </thead>
      <tbody>
        {users.map((user) => (
          <tr key={user.id}>
            <td>{user.name}</td>
            <td>{user.email}</td>
            <td>{user.createdAt}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

## レビュー観点

- 判断が必要な変更がどの行かすぐ分かるか
- formatting 差分が signal を薄めていないか
