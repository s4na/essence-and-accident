# AI Coding Governance

AI によるコーディングで起きがちな「過剰設計」「普通の実装からの逸脱」「レビュー対象ではないはずの定型実装まで毎回レビューしないといけない問題」を抑えるためのスキルです。

このスキルの目的は、AI に自由に設計させることではなく、プロジェクト内で許可された設計空間の中で、最小の差分を高速に実装させることです。

## 何を解決するか

AI はしばしば、単純な機能追加に対しても次のような判断を同時に行ってしまいます。

- 要件を独自解釈する
- アーキテクチャを再設計する
- データモデルを拡張する
- 将来要件を予測する
- 抽象化ポイントを発明する
- その上でコードを書く

人間の開発では、Rails や TypeScript などの既存の常識、プロジェクト固有の慣習、既存コードの置き場所が先にあります。実際に議論したいのは、その制約の中でもまだ判断が必要な部分だけです。

このスキルは、その「判断が必要な部分」と「定型的に実装すべき部分」を分離します。

## 中心となる考え方

実装前に、まず **decision diff** を出します。

Decision diff とは、コード差分そのものではなく、今回新しく発生する設計判断の差分です。

たとえば次のようなものを先に明示します。

- 今回導入する新しい概念
- 変更する不変条件
- DB・schema 変更の有無
- 採用した選択肢
- 棄却した選択肢
- 後戻りが難しい判断
- 既存の慣習から逸脱する箇所

人間はまずここだけを確認します。承認後、AI はその決定から逸脱せずに実装します。

## 資料の役割

このスキルの資料は、AI が実行時に読むものと、人間が理解・レビューするためのものを分けています。

- `SKILL.md`: 作業の開始条件、decision diff、実装、停止、レビューの手順
- `references/`: AI が作業前に読む正規の運用ルール。短い箇条書きで、Rails などの通常の実装方針を記載
- `example/`: `references/` のルールを具体的な要件とコードで理解するための、人間向けの解説

AI に実装を依頼するときは `SKILL.md` と該当する `references/` を使い、`example/` はルールの正本として扱いません。

## 使い方

AI に実装を依頼するときは、最初にコードを書かせず、次のように依頼します。

```text
Before editing code, produce a decision diff only.

Include:
1. Requirement interpretation
2. Existing files/patterns to follow
3. Files expected to change
4. New files
5. DB/schema changes
6. New concepts
7. Invariants and invalid states
8. Alternatives rejected
9. Decisions requiring approval
10. Test plan

Constraints:
- Minimize new concepts, not just lines.
- Read and apply the relevant `references/` before making design choices.
- Implement only current requirements.
- Follow local repository patterns over generic best practices.
- If a new design decision appears during implementation, stop.
```

## レビューで見るべきこと

コードを最初から全部読む前に、まず次を確認します。

- 新しい概念が増えていないか
- DB や不変条件が変わっていないか
- 後戻りしづらい判断が含まれていないか
- 機械的な変更と設計判断が混ざっていないか
- 既存のローカル慣習から逸脱していないか
- `status` などによって二重管理や不正状態が増えていないか
- 削れるファイル、抽象化、状態、schema 変更がないか

目的は「AI に良いコードを書かせる」ことではなく、「AI に判断させてよい範囲を狭くする」ことです。

## コミットの考え方

AI の変更は、ファイル単位ではなく意味単位で分けるのが望ましいです。

例:

1. 必要な場合だけ、機械的整理
2. 承認済みの場合だけ、データモデルや不変条件の変更
3. 振る舞いの追加
4. UI / API の接続
5. テスト

同じコードを複数コミットで何度も書き換えると、レビュー側は同じ意味を何度も追うことになります。可能な限り、既存行を一度だけ変更する計画にします。

## メンテナンス

- AI の判断基準を追加・変更するときは、まず該当する `references/` の箇条書きを更新する。
- Rails、frontend、infrastructure などの通常の実装方針は、該当する reference に短く書く。`SKILL.md` に同じルールを重複して書かない。
- 人間が具体的なケースを理解できるようにする必要がある場合だけ、対応する `example/` を追加・更新する。
- `example/` は reference の文章を箇条書きで再掲する場所ではなく、要件、やらないこと、decision diff、実装例、レビュー観点を説明する場所とする。
- reference の追加・変更・削除時は、`example/README.md` と該当カテゴリの README、影響を受ける個別 example の対応表と内容を確認する。
