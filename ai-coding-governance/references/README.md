# References

このディレクトリは、`ai-coding-governance` スキルが実行時に参照するリファレンスです。

スキル実行時は、まず `SKILL.md` を読み、必要な責務領域に応じてこのディレクトリ内のリファレンスだけを読むことを想定しています。

## 使い分け

- バックエンドのコード変更: [backend.md](./backend.md)
- フロントエンドのコード変更: [frontend.md](./frontend.md)
- インフラのコード変更: [infrastructure.md](./infrastructure.md)
- メタなアーキテクチャ判断: [architecture.md](./architecture.md)

## Examples の位置づけ

[examples/](./examples/) は、人間がリファレンスを読むときに「このルールに従うと実態のあるコードはどう見えるのか」を確認するための補助資料です。

通常のスキル実行では、examples は読まないでください。必要な判断は `SKILL.md` と `references/*.md` で行い、examples はリファレンスの理解を深めるための人間向け補足として扱います。
