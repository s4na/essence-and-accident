# Frontend reference

- 変更前に、対象 component、画面、型、state、test と近隣の実装を確認する。
- 小さな UI state は、既存 component の局所的な state や関数で表現する。
- 複数画面で実際に共有されていない処理のために、generic base、interface、共通 component、状態管理層を新設しない。
- 標準 API と既存依存で要件を満たせるなら、新しい dependency を追加しない。
- 現在の画面要件に必要な分だけ実装し、将来の variant や拡張点を先回りして作らない。
- 美的理由や命名統一だけで、変更対象の周辺コードをリファクタリングしない。
- 仕様上の判断を含む行と、型付けや配線など機械的な行を分ける。
- 既存の state、型、render、test のパターンに合わせ、同じ行を何度も書き換えない。
