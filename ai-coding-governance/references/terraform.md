# Terraform reference

- 変更前に、対象 resource、module、provider、state、variable、workspace と近隣の構成を確認する。
- 既存 resource の設定変更で要件を満たせるなら、新しい managed resource や module を追加しない。
- 一つの利用箇所のために generic module、variable、provider abstraction を作らない。
- variable、secret、configuration は、現在の要件、未設定時の挙動、適用範囲を示してから追加する。
- `plan` で create、update、replace、destroy の差分を確認し、破壊的な変更を暗黙に許容しない。
- 権限は最小限にし、secret を state、ログ、plan 出力へ不用意に露出させない。
- 未要求の環境、provider、retry、scaling、将来用の default を先回りして追加しない。
- 既存の命名、tag、state 管理、適用手順に合わせ、最小差分で変更する。
