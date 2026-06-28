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

### MDローカル本体のパス(2026-06-28 確定)

ユーザーは正式版とβ版の両方をプレイ。最近の主用途はβ版。
**本MOD設計の主参照源はβ版**とする。差分確認時に正式版・GitHub dev版を当たる。

| 種別 | パス | バージョン | HOI4対応 |
|------|------|------------|----------|
| **Steam β版(主)** | `C:\Program Files (x86)\Steam\steamapps\workshop\content\394360\3374271790\` | MD 2.0.0 "Beta Test" | 1.19.* |
| Steam 正式版(副) | `C:\Program Files (x86)\Steam\steamapps\workshop\content\394360\2777392649\` | MD 1.12.2 | 1.17.* |
| GitHub dev版 一時(参考) | `C:\Users\tkmuh\AppData\Local\Temp\claude\md_check\Millennium-Dawn\` | dev | (一時クローン、消失リスクあり) |

派生MOD(参考用):
- Millennium Dawn: Expanded Technologies (MDET) = `3202051246`

descriptor.mod の `replace_path` リストにより、本MODが**置換**できる
パスと**追加のみ可能**なパスが決まる。MD公式が replace 宣言している場所
(`common/units/equipment` 等)は、本MODが上書き衝突しないよう
追加ファイル方式で実装する。

### MDリポジトリ既存実装

調査済みの参考実装(2026-06-28 Steam β版で再確認):

- **アーセナルバードのアーキタイプ**: `mothership_equipment`
  - 場所: `common/units/equipment/MD_plane_airframes.txt`, line 6086
    (variant `mothership_equipment_1` は line 6238)
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

### MDモジュール定義書式(2026-06-28 Steam β版 `3374271790` で確認)

P3「モジュール定義書式・stat一覧・modifier書式」の系統調査結果。
主参照:

- `common/units/equipment/modules/MD_plane_modules.txt`
- `common/units/equipment/modules/MD_ship_modules.txt`
- `common/units/equipment/MD_plane_airframes.txt`
- `common/units/equipment/MD_mtg_ships.txt`

#### 1. モジュール定義で実際に確認できた主要フィールド

航空機・艦船のモジュール定義で繰り返し確認できた基本フィールド:

- `abbreviation`
- `category`
- `sfx`
- `xp_cost`
- `add_stats`
- `build_cost_resources`

用途に応じて使われる任意フィールド:

- `add_equipment_type`
- `allow_mission_type`
- `multiply_stats`
- `parent`
- `can_convert_from`
- `manpower`
- `add_average_stats`
- `dismantle_cost_ic`
- `critical_parts`

確認例:

- 航空機エンジンの基本形:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:22>)
- 上位tierでの `parent` と `can_convert_from`:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:54>)
- アビオニクスでの `manpower = -1` と `module_category` 指定の変換:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:1346>)
- 艦載レールガンでの `add_average_stats` / `dismantle_cost_ic` / `critical_parts`:
  [MD_ship_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_ship_modules.txt:4067>)

#### 2. `add_stats` で実際に使われている stat キー

航空機・艦船モジュール定義から機械抽出したキー:

- `air_agility`
- `air_attack`
- `air_bombing`
- `air_defence`
- `air_ground_attack`
- `air_superiority`
- `anti_air_attack`
- `armor_value`
- `build_cost_ic`
- `carrier_size`
- `carrier_sub_detection`
- `carrier_surface_detection`
- `fuel_consumption`
- `hg_armor_piercing`
- `hg_attack`
- `lg_attack`
- `max_organisation`
- `max_strength`
- `maximum_speed`
- `mines_planting`
- `mines_sweeping`
- `naval_range`
- `naval_speed`
- `naval_strike_attack`
- `naval_strike_targetting`
- `naval_torpedo_hit_chance_factor`
- `night_penalty`
- `reliability`
- `sub_attack`
- `sub_detection`
- `sub_visibility`
- `supply_consumption`
- `surface_detection`
- `surface_visibility`
- `thrust`
- `torpedo_attack`
- `weight`

補足:

- `build_cost_ic` や `fuel_consumption` も `add_stats` 内で直接調整している。
- コメント上は `weather_penalty` が見えるが「doesn't work」と注記されており、
  実際の有効キーとしては扱わない方が安全:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:1288>)

#### 3. `multiply_stats` で実際に使われている stat キー

機械抽出したキー:

- `air_agility`
- `air_attack`
- `air_bombing`
- `air_defence`
- `air_ground_attack`
- `air_range`
- `air_superiority`
- `anti_air_attack`
- `armor_value`
- `build_cost_ic`
- `fuel_consumption`
- `hg_armor_piercing`
- `hg_attack`
- `lg_attack`
- `max_strength`
- `maximum_speed`
- `naval_range`
- `naval_speed`
- `naval_strike_attack`
- `naval_strike_targetting`
- `naval_torpedo_enemy_critical_chance_factor`
- `reliability`
- `sub_attack`
- `sub_detection`
- `sub_visibility`
- `surface_detection`
- `surface_visibility`
- `torpedo_attack`

補足:

- `multiply_stats` は割合補正で、航空機アビオニクスや艦載主砲系で多用される。
- `add_stats` と完全一致ではない。たとえば `air_range` は `multiply_stats` 側で
  観測された。

#### 4. `build_cost_resources` で実際に使われている資源名

機械抽出した資源キー:

- `aluminium`
- `chromium`
- `composites`
- `microchips`
- `steel`
- `tungsten`

補足:

- 少なくとも航空機・艦船モジュール定義では `rubber` は観測されなかった。
- 一方でアーキタイプ本体側の `resources` ブロックでは `rubber` が使われる例がある:
  [MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6113>)

#### 5. `module_count_limit` の書式と既存使用例

書式は2系統確認:

- カテゴリ制限:
  `module_count_limit = { category = <module_category> count < N }`
- 特定モジュール制限:
  `module_count_limit = { module = <module_id> count < N }`

航空機側の複数行記述例:
[MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:175>)

艦船側の1行記述例:
[MD_mtg_ships.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_mtg_ships.txt:235>)

観測できた挙動:

- `allowed_module_categories` で受け入れ可能にしても、`module_count_limit` で
  実装上の上限を別途掛けている。
- 艦船側はカテゴリ上限を1行で大量に並べる書き方が多い。

#### 6. `upgrades` ブロックの仕組み

`upgrades` はアーキタイプ/variant 側にぶら下がる列挙ブロックで、該当装備に
適用できるアップグレード系列を指定している。

航空機の例:

- `plane_bba_radar_upgrade`
- `plane_bba_engine_upgrade`
- `plane_bba_bomb_upgrade`
- `plane_bba_turret_upgrade`

確認例:
[MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:262>)

艦船の例:

- `ship_CM_upgrade`
- `ship_ground_support`
- `ship_VLS_upgrade`
- `ship_gun_upgrade`
- `ship_armor_upgrade`
- `carrier_engine_upgrade`
- `carrier_displacement_upgrade`
- `ship_reliability_upgrade`
- `sub_displacement`
- `sub_sonar_upgrade`
- `sub_stealth_upgrade`
- `sub_engine_upgrade`
- `sub_torpedo_upgrade`

確認例:

- 水上艦:
  [MD_mtg_ships.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_mtg_ships.txt:148>)
- 潜水艦:
  [MD_mtg_ships.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_mtg_ships.txt:5430>)

#### 7. 新カテゴリ受け入れの実装ポイント

新カテゴリを積めるようにする場所はアーキタイプ側の `module_slots` 内
`allowed_module_categories`。

航空機例:
[MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:164>)

艦船例:
[MD_mtg_ships.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_mtg_ships.txt:160>)

補足:

- 実装は「モジュール定義ファイルに新カテゴリのモジュールを書く」だけでは足りない。
- 積載先アーキタイプの `allowed_module_categories` と `module_count_limit` の
  両方を見る必要がある。

#### ローカライズキー命名規則(2026-06-28 Steam β版 `3374271790` で確認)

調査対象:

- `localisation/english/plane_designer_l_english.yml`
- `localisation/japanese/plane_designer_l_japanese.yml`
- `localisation/english/replace/replaced_from_special_projects_l_english.yml`
- `localisation/japanese/replace/replaced_from_special_projects_l_japanese.yml`

##### 1. モジュール本体の表示名・説明

モジュール本体は **`<module_id>` が表示名、`<module_id>_desc` が説明文**。
`_DESC` の大文字サフィックスではなく、小文字 `_desc` が実使用。

確認例:

- `weap_droneswarm_anti_ship_1`:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:8303>)
  → [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:797>)
  / [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:798>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:730>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:731>)
- `fully_autonomous_computer_system_arsenal`:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:7645>)
  → [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:805>)
  / [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:806>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:738>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:739>)

##### 2. 略称(`abbreviation`)は localisation キーではなく定義ファイル内の生文字列

少なくとも Arsenal Bird 系モジュールでは、略称はモジュール定義内
`abbreviation = "..."` で直接書かれており、対応する
`<module_id>_abbreviation` ローカライズキーは上記 yml では確認できなかった。

確認例:

- `fully_autonomous_computer_system_arsenal = { abbreviation = "scsh" }`:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:7645>)
- `weap_droneswarm_anti_ship_1 = { abbreviation = "wdas1" }`:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:8303>)

##### 3. モジュールカテゴリ表示名は `EQ_MOD_CAT_<category>_TITLE`

カテゴリの表示名は `EQ_MOD_CAT_<category>_TITLE` 形式。
今回の調査範囲では `EQ_MOD_CAT_*_DESC` は確認できなかった。

確認例:

- `plane_droneswarm_weapon` → `EQ_MOD_CAT_plane_droneswarm_weapon_TITLE`:
  [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:88>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:720>)
- 汎用の近縁カテゴリ `plane_heavy_special_design`:
  [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:53>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:708>)

補足:

- `fully_autonomous_computer_system_arsenal` のカテゴリは
  `plane_heavy_special_design_arsenal`
  ([MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:7647>))
  だが、この **完全一致の `EQ_MOD_CAT_plane_heavy_special_design_arsenal_TITLE` は
  今回の調査範囲で確認できなかった**。
- その代わり、Arsenal Bird 用スロット名は
  `EQ_MOD_SLOT_fixed_primary_command_operations_TITLE`,
  `EQ_MOD_SLOT_fixed_drone_doc_slot_1_TITLE` などで個別に定義されている:
  [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:27>)
  / [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:28>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:714>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:715>)

##### 4. アーキタイプ・equipment の表示名

アーキタイプ/variant も **`<equipment_id>` が表示名、`<equipment_id>_desc` が説明**。
`_NAME` サフィックスは使われていない。

確認例:

- `mothership_equipment`:
  [MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6086>)
  → [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:542>)
  / [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:600>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:722>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:723>)
- `mothership_equipment_1`:
  [MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6238>)
  → [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:543>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:721>)

##### 5. Special Project 関連

Special Project 名は **`<project_id>` そのまま**でローカライズされている。
今回の確認例 `sp_arsenal_bird` では説明文キーは確認できなかった。

確認例:

- `sp_arsenal_bird` 定義:
  [air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:651>)
- 英語ローカライズ:
  [replaced_from_special_projects_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/replace/replaced_from_special_projects_l_english.yml:253>)
- 日本語ローカライズ:
  [replaced_from_special_projects_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/replace/replaced_from_special_projects_l_japanese.yml:360>)

##### 6. 技術ツリー関連(参考)

技術キーも `tech_id` / `tech_id_desc` の2本構成。

確認例:

- [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:260>)
  / [plane_designer_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/plane_designer_l_english.yml:261>)
- [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:759>)
  / [plane_designer_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/plane_designer_l_japanese.yml:760>)

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

---

## マブラブMOD移植プロジェクト(MD_TSF_Submod)からの転用知見

過去のマブラブ戦術機追加MOD移植プロジェクト(`C:\dev\MD_TSF_Submod`)で
実際に発生した、HOI4/MDモジュール実装一般に再発しうる技術的地雷。
TSF/マブラブ固有の内容(戦術機スロット構成、OS種別等)は対象外とし、
ACM-MDのモジュール追加作業に転用可能なものだけを転記する。
出典: `C:\dev\MD_TSF_Submod\KNOWLEDGE.md`(2026-06-27 棚卸し時点)

### 1. 技術カテゴリの研究速度ボーナスは idea 経由が必須

- **事象**: focus / continuous_focus の `modifier` ブロックに
  `MVLV_tsf_tech = 0.10` のような技術カテゴリ研究速度を直書きすると
  `error.log` に `Unknown modifier` が出て、それ以降のfocus群が
  テックツリーで描画されなくなる
- **原因**: focus/continuous_focusのmodifierは country modifier のみ
  受け付ける。技術カテゴリの研究速度ボーナスはcountry modifierではない
- **対策**: `research_bonus` を持つ hidden idea を作り、
  focusから `idea = X` で付与する

### 2. .gfx ファイル1つあたりの sprite 数は約300個未満を維持する

- **事象**: 1つの.gfxファイルに大量(300個超)のspriteを追加した結果、
  同じ起動セッション内の**他のUI**(開発者コンソールの入力欄等)まで
  描画が壊れた。error.logに目立った警告は出ない
- **原因**: HOI4は1ファイル内のsprite数が一定数を超えるとGUIロードが
  部分的に失敗し、同フォルダ内の他gfxファイルのロードも巻き込まれる
- **対策**: spriteを複数の.gfxファイルに分割する。新規モジュールアイコン
  追加時は、追加先フォルダに既存の大規模.gfxファイルが無いか確認する

### 3. tech定義ファイルで `picture = ...` は無効フィールド

- **事象**: テック側に `picture = GFX_X_medium` を書いても無視され
  全てフォールバックアイコンになる。さらに `Unknown modifier: picture`
  エラーが出るだけでなく、**そのテック内の後続フィールド処理が中断**
  され、`enable_subunits` 等が無視される
- **対策**: テックアイコンは自動命名規則(`GFX_<tech_id>_medium`)に
  従った別.gfxファイルで解決する。未対応フィールドを一括追加すると
  cascadingな動作不良を引き起こすことがあるため要注意

### 4. 新規ルートテックは gridboxtype を手動追加しないとツリーに表示されない

- **事象**: dependenciesの無い新規テックを技術として登録したが、
  テックツリー画面に描画されない(コマンドでの解放は可能)
- **原因**: `countrytechtreeview.gui` はルートテックごとに個別の
  `gridboxtype` を要求する。子テックは自動連結されるがルートは
  描画指示が別途必要
- **対策**: folder内の既存gridbox群の末尾に新規 `gridboxtype` を追加

### 5. モジュールカテゴリのlocalizationキー未定義で生キー表示

- **事象**: 装備デザイナーでモジュール選択時、カテゴリ名が
  `EQ_MOD_CAT_<category>_TITLE` のような生キーのまま表示される
- **対策**: モジュール定義の `category = X` に対応する
  `EQ_MOD_CAT_X_TITLE` / `EQ_MOD_CAT_X_DESC` をlocファイルに追加する

### 6. HOI4識別子(equipment ID / tech ID)にハイフンは使用不可

- **事象**: `F-22`のようなハイフンを含む内部IDを定義すると挙動が不安定
- **対策**: 内部IDはアルファベット・数字・アンダースコアのみ。
  表示名(abbreviation、翻訳テキスト)はハイフン可

### 7. default_modulesのslot category不一致でautosaveクラッシュ

- **事象**: スロットの `allowed_module_categories` と異なるカテゴリの
  モジュールをdefault_modulesに割り当てた結果、装備としては作成できる
  が設計が無効状態になり、autosave時に `Invalid subunit` でクラッシュ
- **対策**: モジュールのcategoryを確認してからdefault_modulesに割り当てる。
  ACM-MDは既存アーキタイプに新カテゴリを追加する方針のため、
  スロットの`allowed_module_categories`への追加と整合性確認は必須工程

### 8. equipmentファイルは `equipments = { }` で囲む

- **事象**: 素体定義ファイルの先頭を `duplicate_archetypes = { }` で
  囲んだ結果、その素体を使う機体が生産解放されるとEXCEPTION_ACCESS_VIOLATION
  (再帰の暴走)でクラッシュした。enable回数0の機体は巻き添えにならず
  「無実」に見えるため誤った比較対象になりやすい
- **対策**: 素体・装備ファイルは必ず `equipments = { }` で囲む。
  静的解析で原因不明の場合は「ファイルを囲む最上位キーワード」を確認する

### 9. localisationファイル名は他MODと衝突しないユニーク名にする

- **事象**: ベースMOD(MD)と同名のlocalisationファイル
  (`equipment_l_japanese.yml`)を作成した結果、自分のファイルが
  優先されてMD側の翻訳がブロックされた
- **対策**: ファイル名に必ずprefixを付ける(ACM-MDなら `acm_` 等)。
  特に `replace/` フォルダ配下は要注意

### 10. 新規作成したlocalisation ymlファイルはBOM付与が必要

- **事象**: 新規作成した`*_l_japanese.yml`がエディタ上で日本語が
  化ける(HOI4自体の動作には影響しないが編集時に支障)
- **原因**: 一般的なツールで新規作成したymlはUTF-8 BOM無しだが、
  既存MD系ymlはBOM有り
- **対策**: locファイル新規作成時はBOM付与を確認する。
  逆に `common/*.txt` はBOM無しで保存すること
  (BOM有だと `Unexpected token` でツリー全消滅の実例あり)

### 11. クラッシュ調査は必ずcrash dump(exception.txt)を最初に確認する

- **事象**: ユーザーの言葉(「○○のあたりで落ちる」)だけを頼りに
  定義ファイルの検証を繰り返す回り道をした。crash dumpを最初に見れば
  一発で原因系統が分かったケースがあった
- **対策**: 必ず以下の順で確認する
  1. `crashes/`最新フォルダの `exception.txt`(スタックトレース)・`meta.yml`
  2. `logs/error.log`(致命的エラー)
  3. `logs/game.log`(クラッシュ直前の挙動)
- **読み方**: `PHYSFS`系のスタックトレース(シンボル無し含む)が出ている場合は
  「文法エラー」ではなく「ファイル/アセットの読込失敗」(欠落・参照切れ)を疑う

### 12. MODアセットの流用はサブディレクトリ単位でコピーする

- **事象**: 元MODの `gfx/models` フォルダを丸ごとコピーした結果、
  同梱されていた無関係なファイル(歩兵モデル等)がベースMOD本体の
  ファイルを上書きし、別タイミングでベースMOD側がクラッシュした
- **対策**: アセット流用時は実際に必要なサブディレクトリのみコピーする。
  コピーする各ファイルの相対パスがベースMOD/HOI4本体に同名で
  存在しないか(=上書きの危険)を事前にチェックする

### 13. module_slots / default_modulesは親素体の継承チェーンに沿って検証する

- **事象**: 派生素体ごとに実際に使えるスロット構成が異なるのに、
  ベース素体(全スロットフル)基準でモジュール割り当てを検証してしまい、
  存在しないスロットへの割り当てがautosaveクラッシュの原因になった
- **対策**: モジュール追加・割り当て時は、対象アーキタイプ/variantの
  親をたどって「自分宣言スロット ∪ 親の有効スロット」を再帰的に確認する。
  ACM-MDは既存MDアーキタイプへの追加が中心のため、対象アーキタイプの
  実際のスロット構成をMD実コードで確認する工程(SPEC.md Phase 1)を
  省略しないこと
