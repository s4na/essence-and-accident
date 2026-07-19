# 同じ行を何度も触らない: churn を減らす

## 要件

商品一覧に価格列を追加し、テストも追加する。

## 悪い進め方

1. table を全面整形する
2. 価格列を追加する
3. lint に合わせて再整形する

## 普通の進め方

既存 table の必要な位置にだけ列を追加し、同じ行を再編集しない。

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
