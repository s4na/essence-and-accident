# Architecture examples

技術スタックに依存しない、AI コーディングの進め方・止まり方・境界設計・レビュー単位・コミット単位に関する例をまとめています。

ここでは個別コードより一段上の、メタなアーキテクチャ判断を扱います。AI にどこまで判断させ、どこで止め、どの単位で人間が承認するかを明示するためのドキュメントです。

## 一覧

- [decision diff と実装の分離](./decision-diff-full-flow.md)
- [local consistency 優先](./local-consistency.md)
- [承認後に逸脱しない](./post-approval-stop.md)
- [意味単位コミット](./meaning-based-commits.md)
- [境界を増やす前のチェック](./boundary-before-abstraction.md)
- [可逆性を優先するアーキテクチャ判断](./reversibility.md)
