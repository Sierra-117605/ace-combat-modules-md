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

## ドキュメント構成(本人向けガイド)

以下のドキュメントを役割別に整理する。
**「本人が読む/編集するもの」と「AIが読み書きする運用ファイル」**を
区別して並べる。

### A. 本人が普段読むもの(プロジェクトの方針・状況)

| ファイル | どんな時に開く | 中身の概要 |
|----------|----------------|------------|
| [`MOD_OVERVIEW.md`](./MOD_OVERVIEW.md) | 「このMODって結局何作るんだっけ?」 | プロジェクトの全体像。最初の地図 |
| [`PLAN.md`](./PLAN.md) | 「何をどうしたいんだっけ?」 | やりたいこと・成功イメージ・対象作品 |
| [`SPEC.md`](./SPEC.md) | 「どう作るんだっけ?」 | 仕様。HOI4/MD側の実装方針 |
| [`TODO.md`](./TODO.md) | 「いま何が進んでて何が残ってる?」 | フェーズ別タスク一覧(P0〜P6)。チェックボックス管理 |
| [`DATABASE.md`](./DATABASE.md) | 「AC作品のどの兵器を実装するか整理されてる場所」 | 兵器ごとの根拠台帳。典拠URL・HOI4実装判定付き |

### B. AI間の引き継ぎ・知見蓄積(本人は時々確認すればOK)

| ファイル | どんな時に開く | 中身の概要 |
|----------|----------------|------------|
| [`HANDOFF.md`](./HANDOFF.md) | 「AI同士で何の判断待ちが起きてる?」 | Claude Code ⇔ Codex 間の申し送り。新しいエントリが先頭 |
| [`KNOWLEDGE.md`](./KNOWLEDGE.md) | 「同じ地雷を二度踏まないための備忘録」 | ハマった事例・既存実装の参照箇所・典拠ルール |
| [`instructions/`](./instructions/) フォルダ | 「Claude Code が Codex に出している作業指示書一覧」 | 未対応の指示書が置かれる。完了したものは `done/` へ移動 |

### C. AI向けの規約(本人は基本触らない)

| ファイル | 役割 |
|----------|------|
| [`CLAUDE.md`](./CLAUDE.md) | Claude Code 向け指示書(プロジェクトルール) |
| [`AGENTS.md`](./AGENTS.md) | Codex 等汎用AIエージェント向け指示書。**CLAUDE.md と内容を常に同期** |
| [`INSTRUCTION_FOR_DATABASE.md`](./INSTRUCTION_FOR_DATABASE.md) | DATABASE.md の書き方ルール。AIが参照する |

### 迷ったらここを見る早見表

- 今やってる作業の状態が知りたい → **TODO.md**
- AIがなぜ止まってるか知りたい → **HANDOFF.md**(先頭が最新)
- どの兵器を扱う予定か → **DATABASE.md**
- このMODが目指すゴール → **MOD_OVERVIEW.md** または **PLAN.md**
- 過去にハマった話 → **KNOWLEDGE.md**

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
