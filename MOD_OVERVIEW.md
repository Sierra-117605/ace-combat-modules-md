# プロジェクト概要

本ドキュメントは、HOI4 mod「Millennium Dawn」向けのACモジュールMODの
全体像を新規参加者(または会話セッション切れ後のClaude/Claude Code)が
最初に把握するための地図である。
詳細は各ドキュメント(PLAN.md / SPEC.md / TODO.md / KNOWLEDGE.md /
DATABASE.md / INSTRUCTION_FOR_DATABASE.md)を参照。

---

## プロジェクト名

ace-combat-modules-md (以下 ACM-MD と表記)
GitHub: https://github.com/Sierra-117605/ace-combat-modules-md

---

## ベースMOD

- **Hearts of Iron IV** (Paradox Interactive)
- 上に乗る **Millennium Dawn: A Modern Day Mod**
  - GitHub: https://github.com/MillenniumDawn/Millennium-Dawn
  - 開発版本体を参照。プレイ環境のSteam版とは差分があり得るため、
    実装時はローカルファイルで再確認すること

---

## 開発者(本人)について

- ネットワークエンジニア(非プログラマ)
- 主要言語: Python、Java(ただしコードを書く能力は無く、AIによる支援が前提)
- ハルシネーション混入を強く嫌う
- ドキュメント体系を重視:
  - PLAN.md(やりたいこと)
  - SPEC.md(仕様)
  - TODO.md(タスク管理)
  - KNOWLEDGE.md(学び・地雷回避)

### 過去MOD経験

- マブラブ戦術機追加MODをMDに移植した経験あり
- 当時の技術ナレッジは TODO P1 で棚卸しし、本リポジトリの
  KNOWLEDGE.md に転記する予定

---

## プレイスタイル(本MODの動機)

- MDでは主に大国(米中露+日本)で覇権狙い
- 陸軍主体、装甲・機械化の物量で押す
- ヘリも状況に応じて使う
- テンプレは深く弄らず、戦争そのものに集中したい
- 「もっとこうしたい」と感じるのは航空・ミサイルとの連携
- 特に近接航空支援(CAS)を多めにやりたい
- 装備の「性能数値」と「見た目・名前」にこだわり、それを大量配備する快感がコア
- 自分の好きな装備を組んだ師団を量産して物量で押し潰すプレイが主軸
- MOD制作の動機は「自分のプレイスタイルに合う道具を増やすこと」

---

## MODの方向性

### コンセプト
エースコンバットシリーズに登場する超兵器・架空兵器の特徴的システムを
Millennium Dawn のモジュール式装備デザイナー上に追加し、
プレイヤーが既存のMD装備に「ACらしさ」を載せて運用できるようにする。

### 採用パラダイム
- **メインはモジュール追加**: 既存アーキタイプのスロットに新カテゴリを足す
- **機体・艦・車両の本体は新規追加せず**、既存品にモジュールで個性を持たせる
- **例外**: 1点物超兵器は Special Projects 経由の専用アーキタイプとして扱う
  (MD既存パターンに倣う。アーセナルバードが先行例)
- **入手条件**: 研究で解放
- **陣営性(オーシア/エルジア等の固有化)**: 一旦無視
- **対象作品**: ACシリーズ全体を視野に入れつつ、1作品に絞って深く作る方針

### 採用しないもの
- AC作品の「通常兵装」(4AAM、6AAM、QAAM、SAAM、マルチロックオン系等)
- 固定設置型超兵器のモジュール化(ストーンヘンジ、エクスキャリバー等)
- 量産戦闘機の本体追加(MDの既存実装で十分)
- 架空車両・実在車両・実在艦のDB化(スコープ外)
- 機体・艦本体のアイコン新規作成
  (アイコン制作スキル無し。既存流用が前提)

---

## 対象作品(11作品)

発表順:
- AC2 (Ace Combat 2)
- AC3 Electrosphere
- AC04 (Shattered Skies)
- AC5 (The Unsung War)
- ACZ (Zero: The Belkan War)
- ACX (X: Skies of Deception)
- AC6 (Fires of Liberation)
- AC Joint Assault
- AH (Assault Horizon)
- AC7 (Skies Unknown)
- AC Infinity (2018年3月サービス終了済み)

スコープ外:
- AC8 (Wings of Theve、未発売)
- AHL (Assault Horizon Legacy)
- AC Xi
- AC Northern Wings
- AC Advance

---

## 制約条件

- 開発者にアイコン作成スキルなし
  - モジュールアイコンは新規が必要だが、サイズが小さく数も少ないので許容
  - 機体・艦・車両本体のアイコンは既存流用必須
- MDはバニラから大幅独自実装のため、推測実装は禁止
  - 実コードを読んでから実装する二段構え:
    Phase 1 調査(Claude Code/Codex) → Phase 2 実装(Claude Code/Codex)
- 性能チューニング: 原作寄りだが、コスト・製造難易度・必要技術で抑制
- 開発作業はPC環境で行う(Claude Code または Codex)
  - スマホ(claude.ai)では戦略・指示書作成・調査の補助のみ

---

## ワークフロー分業

| 環境 | 役割 |
|------|------|
| claude.ai (本会話) | 戦略立案、指示書作成、AC設定の調査補助、PLAN/SPEC/TODO 整理 |
| Claude Code (PC) | 実コード調査、DATABASE.md 埋め、モジュール実装、ファイル編集 |
| Codex (PC) | Claude Code と同じプロジェクトルールに従い、依頼により使い分け |

Claude Code と Codex は基本的に**同じプロジェクトルール**(CLAUDE.md / AGENTS.md)に従う。
両者を使う理由は、片方の使用量上限に達した時にもう片方で作業継続するため。
**同時並行作業は禁止**(コンフリクト防止)。

---

## ドキュメント一覧

- **MOD_OVERVIEW.md**(本書): プロジェクト全体の地図
- **PLAN.md**: やりたいことの整理(動機・方向性をテキストで)
- **SPEC.md**: 仕様(モジュール定義、技術要件)
- **TODO.md**: タスク管理(コンテキストリセットしても再開できる粒度)
- **KNOWLEDGE.md**: 学び・地雷回避ノート
- **DATABASE.md**(後で作成): AC兵器および対象実在兵器のローカル根拠台帳
- **INSTRUCTION_FOR_DATABASE.md**: DATABASE.md 作成のための指示書
- **CLAUDE.md**: Claude Code 向け指示書
- **AGENTS.md**: 汎用AIエージェント(Codex等)向け指示書(内容はCLAUDE.mdと同一)
- **README.md**: リポジトリの入口

---

## 現時点で確定している既存実装の参考点

調査済みの MD コード上の参考実装:

- アーセナルバードのアーキタイプ: `mothership_equipment`
  - 場所: `common/units/equipment/MD_plane_airframes.txt`, line 6086 付近
- アーセナルバードの入手フロー: Special Project `sp_arsenal_bird`
  - 場所: `common/special_projects/projects/air_projects.txt`, line 651 付近
- 戦車デザイナー: NSB形式 (`MD_tank_chassis.txt` + `MD_tank_modules.txt`)
- 航空機デザイナー: BBA形式 (`MD_plane_airframes.txt` + `MD_plane_modules.txt`)
  - CV版あり(艦載機)
- 艦船デザイナー: MtG形式 (`MD_mtg_ships.txt` + `MD_ship_modules.txt`)
- ヘリ独自モジュール: `MD_helicopter_modules.txt`

MD既存装備ファイルの分割:
- ミサイル系: `MD_missiles.txt` / `MD_guided_missiles.txt` /
  `MD_ballistic_missiles.txt` / `MD_sam_missile.txt` /
  `MD_nuclear_missiles.txt`
- 艦種: helicopter_operator, battleship, battle_cruiser, cruiser,
  stealth_destroyer, destroyer, screen_destroyer, stealth_frigate,
  frigate, heavy_frigate, corvette, stealth_corvette, patrol_boat,
  carrier, missile_submarine, attack_submarine, repair_ship, support_ship
