# Architecture examples

技術スタックに依存しない、AI コーディングの進め方・止まり方・境界設計・レビュー単位・コミット単位に関する例をまとめています。

対応する正規ルールは [`../../references/meta.md`](../../references/meta.md) と [`../../references/user-software.md`](../../references/user-software.md) です。

ここでは個別コードより一段上の、メタなアーキテクチャ判断を扱います。AI にどこまで判断させ、どこで止め、どの単位で人間が承認するかを明示するためのドキュメントです。

## 一覧

- [ユーザーとの境界を先に設計する](./user-software-contract.md)
- [decision diff と実装の分離](./decision-diff-full-flow.md)
- [local consistency 優先](./local-consistency.md)
- [承認後に逸脱しない](./post-approval-stop.md)
- [意味単位コミット](./meaning-based-commits.md)
- [境界を増やす前のチェック](./boundary-before-abstraction.md)
- [可逆性を優先するアーキテクチャ判断](./reversibility.md)
