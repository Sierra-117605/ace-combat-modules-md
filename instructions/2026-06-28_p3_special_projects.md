# 指示書: P3-C Special Projects 新規追加手順の精査

- 発行者: Claude Code
- 発行日: 2026-06-28
- 担当: Codex
- ステータス: 未対応
- 優先度: **C**(P3-A / P3-B 完了後に着手)

---

## 目的

本MODが「超大型潜水艦アーキタイプ(シンファクシ級・アリコーン)」や
「mothership_equipment 系 新variant(アイガイオン級空中艦・グレイプニル・
アークバード等)」を実装する場合に必要となる、Steam β版MD の
**Special Projects 新規追加手順** を実コードから抽出する。

参考にすべき既存実装は `sp_arsenal_bird`
(`common/special_projects/projects/air_projects.txt:651` 付近)。

---

## 前提・作業前にやること

1. **git pull** で最新を取り込む(P3-A / P3-B の Codex commit が含まれているはず)
2. 以下を確認:
   - `KNOWLEDGE.md` の以下サブセクション:
     - 「MDリポジトリ既存実装」内の「アーセナルバード入手フロー」
     - 「MDモジュール定義書式」
     - P3-A で追加された「ローカライズキー命名規則」(Special Project 名称キー)
     - P3-B で追加された「アイコン .dds 仕様 + .gfx 登録」(SP 用アイコン)
   - `SPEC.md` 2.2(Special Projects + アーキタイプ追加 方針)、
     6.3(By Blood Alone DLC依存)
   - `DATABASE.md` 種別①(アリコーン・シンファクシ級・アーセナルバード・
     アイガイオン関連エントリの MD既存実装欄)
   - `HANDOFF.md`「種別③艦本体: 超大型潜水艦アーキタイプ案で確定」エントリ

---

## 作業内容

### 1. 調査対象パス

Steam β版 `3374271790` の以下:

- `common/special_projects/projects/` 配下(全ファイル)
- `common/special_projects/project_tags/` 配下
- `common/special_projects/prototype_rewards/` 配下
- `common/special_projects/specialization/` 配下
- 連動する localisation キー(P3-A の成果と合わせて確認)
- 連動するアイコン(P3-B の成果と合わせて確認)

### 2. 抽出すべき情報

以下を `KNOWLEDGE.md`「MDモジュール定義書式」セクション直下に **新サブセクション
「Special Projects 新規追加手順」** として追加する:

#### (a) Special Project ファイルの基本構造
- 既存 `sp_arsenal_bird` を題材に、SP定義の必須フィールド一覧
  (`prerequisites`, `allowed`, `enable_equipments`, `enable_modules`,
  `breakthrough_cost`, `specialization`, `ai_will_do` 等)
- 任意フィールドと使い分け

#### (b) Special Project から装備・モジュールを解放する仕組み
- `enable_equipments` で variant を解放する書式と実例
- `enable_modules` で個別モジュールを解放する書式と実例
- `sp_arsenal_bird` での `enable_equipments = { mothership_equipment_1 }` と
  `enable_modules = { weap_droneswarm_fighter_1 ... }` の具体パターン

#### (c) prototype_rewards / specialization の仕組み
- prototype_reward の追加方法
- specialization(専門化)の追加方法
- これらが本MOD実装で必要になるか/省略可能かの判定材料

#### (d) project_tags の使い方
- タグ定義の場所と書式
- 既存タグの再利用 vs 新規タグ追加の判断基準

#### (e) DLC依存と AI 挙動
- `allowed = { has_dlc = "By Blood Alone" }` 等の DLC ガード
- `ai_will_do` の書式(国別優先度、条件分岐)
- 本MOD実装で「特定国だけが Special Project を着手する」設計が可能かの確認

#### (f) Special Project に紐付くアイコン・ローカライズキー(統合)
- P3-A / P3-B で抽出した命名規則を、Special Project 文脈に適用した
  実例(`sp_arsenal_bird` の表示名・説明文・アイコンの命名)

### 3. 加えて記録すべき本MOD設計向けの示唆

KNOWLEDGE.md 末尾に「本MOD向け Special Project 追加設計メモ」として、
以下のサンプル設計案を 1-3 段落で書く:

- **超大型潜水艦** 用 Special Project 設計案
  (例: `sp_super_submarine` を追加し、`enable_equipments` で
  シンファクシ級・アリコーン variant を解放、`prerequisites` で
  `sp_nuclear_engines` 等を要求)
- **mothership_equipment 系 新variant 用** Special Project 設計案
  (アイガイオン級・グレイプニル・アークバード等を `mothership_equipment_2`,
  `mothership_equipment_3` ... として variant 化する場合、各 SP の構造案)

### 4. 報告と更新先

- `KNOWLEDGE.md` に「Special Projects 新規追加手順」を追加
- `SPEC.md` 3.6 から **「Special Projects への新規プロジェクト追加手順」**
  の行を削除し、「3.9 Special Projects 新規追加手順(確定)」サブセクションを
  追加。本MODでの想定実装(超大型潜水艦SP、mothership 系新variant SP)を
  3-5行で記述
- `TODO.md` P3 該当チェックボックス
  「[ ] [Claude Code/Codex] Special Projects の新規追加手順を確認」を `[x]` に更新
- `TODO.md` P3 全体が完了したことを示すため、最後に
  「[ ] [Claude Code/Codex] SPEC.md の「要調査」項目を「確定」に更新」を確認
  (P3-A〜C で各サブセクションを順次「確定」化しているため、最終的にこの
  チェックも `[x]` にする)
- `HANDOFF.md` 冒頭に `[Codex → Claude Code] P3-C Special Projects 追加手順
  調査結果` + 「P3 全完了」として報告

---

## 完了条件

- `KNOWLEDGE.md` に上記 (a) ~ (f) の情報が、**実例(ファイルパス + 行番号 +
  抜粋)** とともに記載されている
- 「本MOD向け Special Project 追加設計メモ」が記載されている
- `SPEC.md` 3.9 が確定状態で記載されている
- `TODO.md` の P3 Special Projects 項目と「SPEC.md の要調査項目を確定に更新」が
  ともに `[x]` になっている
- これにより **P3 SPEC精査フェーズ全体が完了** となる

---

## 完了後の手順

1. commitメッセージに本指示書ファイル名を含める
   例: `[Codex] P3-C Special Projects 追加手順 (ref: instructions/2026-06-28_p3_special_projects.md)`
2. 本ファイルを `instructions/done/` に移動して commit
3. `HANDOFF.md` を更新(P3 全完了を明示し、次フェーズ候補として
   「P4 本命作品選定(本人判断)」「種別⑤実在機」を提示)
4. `git push` してから手放す
5. **P3 全完了で一旦停止**。次の指示書は本人または Claude Code から発出を待つこと

---

## 判断に迷ったら止まって質問すべきケース

- Special Project の仕組みが MD で独自拡張されており、HOI4 標準と大きく
  乖離している場合(本MODでどう追従するか本人判断)
- `sp_arsenal_bird` 以外の MD独自 Special Project 群に重要な実装パターンが
  あり、本MOD設計に影響しそうな場合
- prototype_rewards / specialization が本MOD実装で必須か否か、判断材料が
  乏しい場合
