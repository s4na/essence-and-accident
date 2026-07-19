# Backend reference

- 変更前に、対象の domain model、API、controller、worker、job、database、test と近隣のバックエンド実装を確認する。
- 既存の domain layer や handler に収まる小さな振る舞いは、まず既存の責務へ置く。
- API の小さな変更は、既存の request、response、serializer、error の形式に合わせる。
- 既存の事実、association、timestamp、query から導出できる状態を、別の status や boolean として保存しない。
- 現在の要件で足りるなら、新しい table、column、layer、service、job、queue、state machine を追加しない。
- 新しい責務の置き場所に迷ったら、抽象化や専用 package を先に作らず、既存ファイルの近くで完結できるかを確認する。
- 入力値、認可、query 条件、transaction、例外処理は、最小差分でも既存の安全なパターンに合わせる。
- 非同期化、retry、cache、feature flag は、現在の要件と失敗特性から必要性を示してから導入する。
- テストは既存の model、request、controller、integration、system test の配置とアサーションに合わせる。
