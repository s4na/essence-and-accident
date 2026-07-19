# 同じ行を何度も触らない: churn を減らす

## 要件

商品一覧に価格列を追加し、テストも追加する。

## やらないこと

- table を全面整形しない
- 価格列追加の前後で同じ行を何度も書き換えない
- refactor、実装、cleanup を同じ差分に混ぜない

## decision diff

- 新しい概念: なし
- 採用する実装: 既存 table の必要な位置にだけ列を追加する
- 棄却する実装: column 定義 DSL や table abstraction を作る

## 普通の実装の全体像

```tsx
// app/products/ProductTable.tsx
export function ProductTable({ products }: { products: Product[] }) {
  return (
    <table>
      <thead>
        <tr>
          <th>商品名</th>
          <th>価格</th>
        </tr>
      </thead>
      <tbody>
        {products.map((product) => (
          <tr key={product.id}>
            <td>{product.name}</td>
            <td>{product.priceLabel}</td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

## レビュー観点

- 同じ意味を複数コミットで追わせていないか
- refactor / implementation / cleanup が同じ行に重なっていないか
- 実装に必要な行だけを触っているか
