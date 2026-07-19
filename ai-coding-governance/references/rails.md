# Rails reference

- 変更前に、対象のモデル、controller、view、serializer、migration、test とその周辺にある既存の書き方を確認する。
- 既存モデルに収まる小さな振る舞いは、まず既存モデルの scope やメソッドで表現する。
- controller は HTTP の入力・認可・応答に集中させ、既存の責務配置にない新しい layer を作らない。
- 既存 endpoint の小さなフィールド追加は、既存の response / serializer / view の形式に合わせる。
- 既存の association、timestamp、値、query から導出できる状態のために、status、boolean、enum を新設しない。
- 現在の要件だけで足りるなら、新しい table、column、nullable field、STI、継承階層を追加しない。
- 既存のローカル規約にない service object、concern、callback、background job、feature flag、state machine を導入しない。
- 新しい責務の置き場所に迷ったら、抽象化を先に作らず、既存ファイルの近くで完結できるかを確認する。
- 入力値、認可、query 条件、例外処理は、最小差分でも既存の安全なパターンに合わせる。
- テストは既存のモデル、controller、request、system test の配置とアサーションの書き方に合わせる。
