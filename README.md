# ace-combat-modules-md

HOI4 mod **Millennium Dawn** 向けの、エースコンバット由来モジュール追加MOD
**ACM-MD** の開発リポジトリ。

開発初期段階のため、現時点ではドキュメント類のみ。実装ファイルは
DATABASE.md 完成後、本命作品の選定後に追加されていく。

---

## プロジェクトの目的

エースコンバットシリーズに登場する超兵器・架空兵器の特徴的システムを、
Millennium Dawn のモジュール式装備デザイナー上に追加し、
プレイヤーが既存のMD装備に「ACらしさ」を載せて運用できるようにする。

詳細は [`MOD_OVERVIEW.md`](./MOD_OVERVIEW.md) を参照。

---

## ドキュメント構成

| ファイル | 内容 |
|----------|------|
| [`MOD_OVERVIEW.md`](./MOD_OVERVIEW.md) | プロジェクト全体の地図 |
| [`PLAN.md`](./PLAN.md) | やりたいことの整理 |
| [`SPEC.md`](./SPEC.md) | 仕様 |
| [`TODO.md`](./TODO.md) | タスク管理 |
| [`KNOWLEDGE.md`](./KNOWLEDGE.md) | 学び・地雷回避ノート |
| [`DATABASE.md`](./DATABASE.md) | 兵器の根拠台帳(本リポジトリ初期版には未収録、後で作成) |
| [`INSTRUCTION_FOR_DATABASE.md`](./INSTRUCTION_FOR_DATABASE.md) | DATABASE.md 作成のための指示書 |
| [`CLAUDE.md`](./CLAUDE.md) | Claude Code 向け指示書 |
| [`AGENTS.md`](./AGENTS.md) | 汎用AIエージェント(Codex等)向け指示書 |

---

## 開発フロー

1. **戦略・指示書作成**: claude.ai(ブラウザ/モバイル)
2. **実コード調査・実装**: Claude Code または Codex(PC)
   - 両者は同じプロジェクトルール(CLAUDE.md / AGENTS.md)に従う
   - 同時並行作業は禁止(コンフリクト防止)

---

## 初回セットアップ手順(本人向け)

GitHub上にリポジトリ作成済みであることを前提とする:
https://github.com/Sierra-117605/ace-combat-modules-md

ローカル(PC)で以下を実行:

```bash
# 任意の作業ディレクトリへ移動
cd /path/to/workspace

# このZIPの内容を展開済みフォルダ ace-combat-modules-md/ に入り
cd ace-combat-modules-md

# Git初期化
git init
git branch -M main

# 全ファイルを追加してコミット
git add .
git commit -m "Initial commit: project documentation and agent instructions"

# リモート追加して push
git remote add origin https://github.com/Sierra-117605/ace-combat-modules-md.git
git push -u origin main
```

push完了後、Claude Code または Codex をリポジトリディレクトリで起動する。
エージェントは CLAUDE.md / AGENTS.md を読んで自動的にプロジェクトルールに従う。

---

## ベースMOD

- [Millennium Dawn (GitHub)](https://github.com/MillenniumDawn/Millennium-Dawn)
- Steam Workshop 版を実プレイ環境とする

---

## ライセンス

未設定(後で決定)
