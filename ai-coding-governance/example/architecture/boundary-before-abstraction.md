# 境界を増やす前のチェック: 抽象化ではなく責務境界を見る

## 要件

注文一覧で、注文ごとの「表示用合計金額」を出したい。

## やらないこと

- `OrderPresentationLayer` を新設しない
- 全画面共通の presenter 基底を作らない
- domain service / application service / adapter の層を増やさない

## decision diff

- 新しい境界: なし
- 採用する実装: 既存の表示境界で format する
- 棄却する実装: 1フィールドの表示都合でアーキテクチャ層を追加する

## 普通の実装の全体像

```text
Controller: 既存どおり一覧データを渡す
View / Component: 既存 helper で合計金額を表示する
Model: 新しい責務は追加しない
DB: 変更しない
```

```erb
<!-- app/views/orders/index.html.erb -->
<td><%= number_to_currency(order.total_amount, unit: "円", precision: 0) %></td>
```

## レビュー観点

- 境界を増やす理由が「現在要件」から説明できるか
- 既存境界に自然に収まる処理を別層へ逃がしていないか
- 抽象化によってレビュー対象ファイルが増えていないか
