# KNOWLEDGE.md — 学び・地雷回避ノート

本ドキュメントは、ACM-MD プロジェクトで得た知見・地雷を蓄積する。
目的は「一度ハマったことには二度とハマらない」こと。

## 記入ルール

- 新しい知見は **日付 + 簡潔なタイトル** で追記する
- 構成は「事象 → 原因 → 対策」の3点セット
- 出典(URL、ファイルパス+行番号、コマンド出力等)を可能な限り残す
- 推測は禁止。確認できた事実だけを書く
- ハルシネーション混入を防ぐため、AIに「KNOWLEDGE.md を見て」と指示して
  既存知見を必ず参照させる

---

## 既存知見の参照先

### 過去プロジェクト

- **マブラブ戦術機追加MODのMD移植経験あり**
  - 当時、claude.ai を使って実装した
  - 過去会話メモリには事実のみあり、技術詳細は本セッションに記録なし
  - TODO P1 で過去リポジトリ・ファイルから知見を回収する予定
  - 回収できたら本ドキュメントに転記する

### MDリポジトリ既存実装

調査済みの参考実装(2026-06-27 時点):

- **アーセナルバードのアーキタイプ**: `mothership_equipment`
  - 場所: `common/units/equipment/MD_plane_airframes.txt`, line 6086 付近
  - 性質: `is_archetype = yes`, `is_buildable = no`, `is_convertable = yes`
  - 特徴的スロット: `fixed_primary_command_operations`,
    `fixed_drone_doc_slot_1`〜`fixed_drone_doc_slot_5`,
    `engine_type_slot`(plane_nuclear_eight 必須), `avionics_type_slot`,
    `wingform_type_slot`, `special_slot_type_1`
  - 性能ベース: `air_range = 15000`, `build_cost_ic = 500`,
    レアリソース必須(chromium / microchips / composites)
- **アーセナルバード入手フロー**: Special Project `sp_arsenal_bird`
  - 場所: `common/special_projects/projects/air_projects.txt`, line 651 付近
  - DLC要件: `has_dlc = "By Blood Alone"`
  - 前提研究: `sp_nuclear_engines`, `sp_fully_autonomous_fighters`,
    `sp_fully_autonomous_bombers`
  - AI挙動: 米・ソ・中・仏など大国が優先的に着手
- **アーセナルバード関連の既存モジュール**(例):
  - `fully_autonomous_computer_system_arsenal`
  - `weap_droneswarm_anti_ship_1`
  - 場所: `common/units/equipment/modules/MD_plane_modules.txt`

### MDのモジュール式デザイナー利用状況

- 戦車: NSB形式 (`MD_tank_chassis.txt` + `MD_tank_modules.txt`)
- 航空機: BBA形式 (`MD_plane_airframes.txt` + `MD_plane_modules.txt`)
  - CV版(艦載機)あり
- 艦船: MtG形式 (`MD_mtg_ships.txt` + `MD_ship_modules.txt`)
- ヘリ独自モジュール: `MD_helicopter_modules.txt`

### MDの艦種一覧(2026-06-27 時点)

`common/units/MD_naval_units.txt` で定義されている艦種:
helicopter_operator, battleship, battle_cruiser, cruiser,
stealth_destroyer, destroyer, screen_destroyer, stealth_frigate,
frigate, heavy_frigate, corvette, stealth_corvette, patrol_boat,
carrier, missile_submarine, attack_submarine, repair_ship, support_ship

**注意**: 専用の揚陸艦(LHD/LPD)艦種は無い。輸送扱いになっている可能性がある。

### MDのミサイル系装備の分割

- `MD_missiles.txt`(汎用ミサイル)
- `MD_guided_missiles.txt`(誘導ミサイル。巡航ミサイル=対艦兵装の主軸)
- `MD_ballistic_missiles.txt`(弾道ミサイル)
- `MD_sam_missile.txt`(地対空ミサイル、243行と比較的薄め)
- `MD_nuclear_missiles.txt`(核ミサイル)

---

## 典拠の優先順位(プロジェクトルール)

### 超兵器分類

**公式(Project Aces, acecombat.jp の ACES CHRONICLE)を最優先**とする。
- https://acecombat.jp/en/chronicle/weapons/
- acecombat.wiki.gg の Disputed セクションの独自判定は採用しない

### それ以外のAC作品内の兵器情報

1. acecombat.wiki.gg
2. acecombat.fandom.com
3. acecombat.jp/en/chronicle/weapons/

### 登場機体クロスチェック

- dic.nicovideo.jp/t/a/エースコンバットシリーズの登場機体

### 実在兵器

- 該当国の公式資料 > Jane's等 > 英語版Wikipedia

---

## 確定済みのハルシネーション注意点

claude.ai 側で実際に発生したハルシネーション事例を残す(再発防止のため):

### 2026-06-27 アリコーンの兵装に関する誤認

- **事象**: アリコーンの主兵装として「MPBM(Multi-Purpose Burst Missile)」を
  挙げた
- **事実**: アリコーンの主兵装は**大型レールキャノン + 2基のレールガン砲塔**。
  MPBMはアリコーンに搭載されていない
- **典拠**: https://acecombat.wiki.gg/wiki/Alicorn
- **対策**: AC設定を語る際は必ず acecombat.wiki.gg を当たること

### 2026-06-27 ストーンヘンジの設計変遷の誤認

- **事象**: 「ストーンヘンジは元々移動式として企画されたが、キャパシタが
  技術的に実現不可能だったため固定式となった」と記述した
- **事実**: ストーンヘンジ施設全体は最初から固定式。Wiki上の "locomotive"
  に関する記述は**個別の砲(turret)を機関車式・自走可能にする計画**を
  指していて、施設全体を移動式にする話ではない
- **典拠**: https://acecombat.wiki.gg/wiki/Stonehenge_(Strangereal)
- **対策**: 主語を取り違えないよう、原文を慎重に読む。
  特に "weapons" と "facility" の区別に注意

### 2026-06-27 COFFINシステムをAC04由来と誤認した可能性

- **事象**: AC04の架空兵器候補に「COFFINシステム」を挙げた
- **事実**: COFFIN (Connection For Flight INterface) の初出作品は要確認。
  AC3 Electrosphere との関連が深い可能性がある
- **対策**: DATABASE.md 作成時に Claude Code/Codex で
  COFFIN の初出作品・登場作品を acecombat.wiki.gg で確定する

### 2026-06-27 公式超兵器分類とWiki判定の混同

- **事象**: 「アーセナルバードは超兵器ではない」と記述した(Wiki側の
  Disputed判定をそのまま採用してしまった)
- **事実**: 公式(Project Aces, acecombat.jp の2025年11月更新)では
  aerial warships(XB-0, Sphyrna, Aigaion, Kottos, Gyges, Gleipnir,
  Arsenal Birds)を超兵器として認定している。
  acecombat.wiki.gg はこれに対して Disputed として独自判定を出しているが、
  本プロジェクトでは**公式判定を優先する**
- **典拠**:
  - https://acecombat.jp/en/chronicle/weapons/(公式)
  - https://acecombat.wiki.gg/wiki/Superweapon(Disputed セクション参照)
- **対策**:
  - 超兵器分類は公式(acecombat.jp の ACES CHRONICLE)を一次優先とする
  - acecombat.wiki.gg の Disputed リストにあるものでも、
    公式が超兵器と認定していれば本DBでは「超兵器」として扱う
  - DATABASE.md の種別判定もこのルールに従う

---

## MD実コード調査時のTips

(現時点で記載できる確定事項のみ)

### grep の基本

- 全文検索の起点: `common/` ディレクトリ配下
- `grep -rin "keyword" common/` で大文字小文字を無視してファイル全検索
- 行番号付きで出力するため、Tips/ナレッジに残しやすい

### git sparse-checkout(モバイル等で軽量clone時)

- リポジトリが大きいため、必要なディレクトリだけ取得すると速い
- 例: `git clone --depth 1 --filter=blob:none --sparse <url>` →
  `git sparse-checkout set common`

### ファイルサイズの目安(2026-06 時点の MD GitHub 開発版)

| ファイル | 行数 |
|----------|------|
| `MD_mtg_ships.txt` | 約6926 |
| `MD_plane_airframes.txt` | 約6251 |
| `MD_naval_units.txt` | 約339 |
| `MD_air_units.txt` | 約348 |
| `MD_sam_missile.txt` | 約243 |
| `MD_missiles.txt` | 約85 |

**注意**: GitHub開発版とSteam版で差分がある可能性あり。
実装作業時はプレイ環境のローカルファイルで再確認すること。

---

## 今後追記される予定の項目

(空欄、知見が溜まり次第追加)

- HOI4 modのアイコン .dds 仕様
- モジュールカテゴリ定義の書式
- ローカライゼーションの YAML 仕様
- HOI4 起動時のエラーログの読み方
- MD 固有の特殊な挙動
- マブラブMOD移植時の知見(P1で回収後)
