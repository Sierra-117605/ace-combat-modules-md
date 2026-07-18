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

#### アイコン .dds 仕様 + .gfx 登録(2026-06-28 Steam β版 `3374271790` で確認)

調査対象:

- `interface/plane_modules_icons.gfx`
- `interface/mdult_air_bba.gfx`
- `interface/special_projects/MD_project.gfx`
- `interface/military_industrial_organization/zMD_military_industrial_icons.gfx`
- `gfx/interface/technologies/0_modules/plane_builder/`
- `gfx/interface/special_project/project_icons/`
- `gfx/interface/military_industrial_organization/department_icons/`

##### 1. `.dds` の物理仕様は用途ごとに複数系統ある

Steam β版実ファイルをヘッダ確認した結果、単一規格ではなく **用途別に複数サイズ**
が共存している。

主な観測結果:

- **航空機モジュール系(`plane_builder`)**
  - 最多: `76x42`, `pfFlags=65`, `rgbBits=32`, `RGBA` あり, `mipmaps=1`
    → 264件
  - 次点: `64x64`, `pfFlags=65`, `rgbBits=32`, `RGBA` あり, `mipmaps=1`
    → 113件
  - 一部旧規格: `76x42`, `fourCC='DXT1'` → 34件
  - 例外: `76x42`, `fourCC='DXT5'` → 1件
- **Special Project アイコン(`project_icons`)**
  - 最多: `206x106`, `pfFlags=65`, `rgbBits=32`, `RGBA` あり, `mipmaps=1`
    → 119件
  - 少数例外: `210x106`, `206x106`(24bit), `188x112`
- **MIO 部門アイコン(`department_icons`)**
  - 最多: `48x48`, `pfFlags=65`, `rgbBits=32`, `RGBA` あり, `mipmaps=1`
    → 30件

補足:

- `pfFlags=65` のサンプルは `RMask=16711680 / GMask=65280 / BMask=255 /
  AMask=4278190080` で、32bit RGBA系を確認。
- `mothership_equipment_1_medium` が参照する `B6.dds` は `144x68`,
  `fourCC='DX10'`, `DXGI=91`, `mipmaps=1`。通常のモジュールカードとは別系統。

##### 2. 実サンプル

- モジュール用(長方形, 76x42, RGBA32):
  `ASM1.dds`
  → [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2294>)
- モジュール用(長方形, 76x42, RGBA32):
  `fully_autonomous_computer_system_heavy.dds`
  → [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2275>)
- equipment中サイズ画像(144x68, DX10ヘッダ):
  `B6.dds`
  → [mdult_air_bba.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/mdult_air_bba.gfx:858>)
- Special Project 用(206x106, RGBA32):
  `sp_aircraft_experimentation.dds`
  → [MD_project.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/special_projects/MD_project.gfx:280>)
- MIO 用(48x48, RGBA32):
  `arsenal_bird_equipment.dds`
  → [zMD_military_industrial_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/military_industrial_organization/zMD_military_industrial_icons.gfx:208>)

##### 3. `.gfx` の sprite 登録書式

基本形は次の通り:

```txt
spriteType = {
    name = "GFX_<sprite_name>"
    texturefile = "gfx/interface/<path>.dds"
}
```

確認例:

- `spriteType` 小文字:
  [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2274>)
- `SpriteType` 先頭大文字:
  [mdult_air_bba.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/mdult_air_bba.gfx:858>)

補足:

- MD実装では `spriteType` と `SpriteType` の両表記が共存している。
- 本MODでは混乱防止のため、既存追加先に合わせるか、少なくとも同一ファイル内で
  表記を統一した方が安全。

##### 4. どの `.gfx` に何を置いているか

- **モジュールアイコン**:
  `interface/plane_modules_icons.gfx`
  - `GFX_EMI_<module_id>` 命名を使用
  - 例:
    [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2275>)
    / [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2294>)
- **airframe / equipment の中サイズ画像**:
  `interface/mdult_air_bba.gfx`
  - `GFX_<equipment_id>_medium`
  - 例:
    [mdult_air_bba.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/mdult_air_bba.gfx:858>)
- **Special Project アイコン**:
  `interface/special_projects/MD_project.gfx`
  - `GFX_sp_<project_name>`
  - 例:
    [MD_project.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/special_projects/MD_project.gfx:280>)
- **MIO アイコン**:
  `interface/military_industrial_organization/zMD_military_industrial_icons.gfx`
  - `GFX_military_industrial_organization_<equipment_key>`
  - 例:
    [zMD_military_industrial_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/military_industrial_organization/zMD_military_industrial_icons.gfx:208>)

##### 5. モジュール定義側からの参照

今回確認した `MD_plane_modules.txt` / `MD_ship_modules.txt` には、**モジュール定義内の
`picture = ...` は観測されなかった**。

その一方で、対象モジュールに対応する `GFX_EMI_<module_id>` が
`plane_modules_icons.gfx` に登録されている。

確認例:

- モジュール定義:
  [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:7645>)
  / [MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:8303>)
- 対応 sprite:
  [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2275>)
  / [plane_modules_icons.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/plane_modules_icons.gfx:2294>)

推定:

- **モジュールアイコンは `GFX_EMI_<module_id>` の自動命名規則で解決されている
  可能性が高い**。これは「モジュール定義に `picture` が無い」ことと、
  `.gfx` 側の命名一致からの推定であり、直接仕様書で確認したわけではない。

##### 6. アーキタイプ・variant のアイコン指定

アーキタイプ本体では `picture` と `sprite` の両方が使われる例を確認。

確認例:

- `mothership_equipment`:
  [MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6090>)
  / [MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6101>)
- `convoy`:
  [MD_convoys.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_convoys.txt:5>)
  / [MD_convoys.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_convoys.txt:7>)

観測できたこと:

- `picture = archetype_*` は装備カード/設計画面寄りの画像参照
- `sprite = <short_name>` は別系統の sprite 参照
- `mothership_equipment_1` 側では、今回確認範囲に `picture` / `sprite` の再指定はなく、
  `archetype = mothership_equipment` と `module_slots = inherit` を使っている
  ([MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6238>))

##### 7. Special Project のアイコン参照

Special Project 側は `icon = GFX_sp_*` を明示指定し、`.gfx` で対応 sprite を登録する。

確認例:

- Project 定義:
  [air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:654>)
- sprite 登録:
  [MD_project.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/special_projects/MD_project.gfx:280>)

##### 8. 本MOD実装時の注意

- `plane_modules_icons.gfx` は `spriteType` が 568 件、`mdult_air_bba.gfx` は
  524 件とかなり大きい。過去サブMOD知見の「1ファイル約300未満維持」は、
  **MD本体が既に大規模でも、本MOD側では分割を優先する**方針で守った方がよい。
- 本MODでは `interface/acm_plane_modules_icons.gfx`,
  `interface/acm_airframes_medium.gfx`, `interface/special_projects/acm_projects.gfx`
  のように **用途別に分けて追加**するのが無難。

#### Special Projects 新規追加手順(2026-06-28 Steam β版 `3374271790` で確認)

調査対象:

- `common/special_projects/projects/air_projects.txt`
- `common/special_projects/projects/zz_netherlands_special_projects.txt`
- `common/special_projects/project_tags/tags.txt`
- `common/special_projects/prototype_rewards/generic_air_prototype_rewards.txt`
- `common/special_projects/specialization/specializations.txt`
- `common/special_projects/special_projects_documentation.md`

##### 1. `sp_arsenal_bird` で確認できた project 定義の基本構造

`sp_arsenal_bird` には、以下の項目がこの順で入っている:

- `allowed`
- `specialization`
- `icon`
- `project_tags`
- `available`
- `breakthrough_cost`
- `prototype_time`
- `complexity`
- `resource_cost`
- `project_output`
- `generic_prototype_rewards`
- `ai_will_do`

参照:
[air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:651>)

実例:

- `allowed = { has_dlc = "By Blood Alone" }`
- `specialization = specialization_air`
- `icon = GFX_sp_aircraft_experimentation`
- `project_tags = sp_tag_avionics_aeronautics`
- `available` 内で前提 project 完了を要求
- `project_output` 内で equipment と module を解放
- `ai_will_do` 内で特定国に加点

補足:

- `available` は常時表示 project では省略される例がある
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:4>))
- `unique_prototype_rewards` は `sp_arsenal_bird` には無いが、
  `sp_dutch_super_bus_project` にはあるため、既存実装上は任意
  ([zz_netherlands_special_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/zz_netherlands_special_projects.txt:52>))

##### 2. Special Project から装備・モジュールを解放する仕組み

- 装備 variant 解放は `project_output.enable_equipments = { <equipment_id> }`
- 個別 module 解放は `project_output.enable_equipment_modules = { <module_id> }`

`sp_arsenal_bird` 実例:

- `enable_equipments = { mothership_equipment_1 }`
- `enable_equipment_modules = { weap_droneswarm_fighter_1 ... fully_autonomous_computer_system_arsenal }`

参照:
[air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:681>)

補足:

- エンジン標準の書式説明でも `enable_equipment_modules` と `enable_equipments`
  が別項目として説明されている
  ([special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:218>),
  [special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:248>))
- equipment だけを解放する短い実例は `sp_dutch_super_bus_project`
  ([zz_netherlands_special_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/zz_netherlands_special_projects.txt:32>))

##### 3. `prototype_rewards` / `specialization` の仕組み

- `generic_prototype_rewards` は project 側から reward ID を列挙して使う
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:694>))
- `generic_air_prototype_rewards.txt` では、各 reward が `token` と
  `add_breakthrough_progress` などの効果を持つ
  ([generic_air_prototype_rewards.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/prototype_rewards/generic_air_prototype_rewards.txt:5>),
  [generic_air_prototype_rewards.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/prototype_rewards/generic_air_prototype_rewards.txt:140>))
- `unique_prototype_rewards` を作る場合は project 内に別ブロックを追加する
  ([zz_netherlands_special_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/zz_netherlands_special_projects.txt:52>))
- Steam β版の実ファイルで確認できた specialization は
  `specialization_nuclear`, `specialization_naval`, `specialization_air`,
  `specialization_land` の4系統
  ([specializations.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/specialization/specializations.txt:4>))
- 新規 specialization のひな形はローカル文書にあるが、背景画像・blueprint・
  facility model まで連動する
  ([special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:502>))

本MOD向け判断材料:

- 超兵器追加だけなら、まずは既存 `specialization_air` / `specialization_naval`
  の再利用で始められる
- `generic_prototype_rewards` も既存プールの再利用で開始可能
- 新規 specialization は画像・施設モデルまで増えるため、現段階ではコストが重い

##### 4. `project_tags` の使い方

- 定義場所は `common/special_projects/project_tags/tags.txt`
  ([tags.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/project_tags/tags.txt:4>))
- 既存再利用候補:
  - `sp_tag_naval_vehicles`
  - `sp_tag_avionics_aeronautics`
  - `sp_tag_aircraft_engines`
  - `sp_tag_pilotless_vehicles`
  ([tags.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/project_tags/tags.txt:13>))

本MOD向け判断材料:

- 超大型潜水艦は `sp_tag_naval_vehicles`
- 空中艦・無人機寄り超兵器は `sp_tag_avionics_aeronautics` または
  `sp_tag_pilotless_vehicles`
- 既存タグで意味が通る間は新規タグ追加を避けた方が差分が小さい

##### 5. DLC依存と AI 挙動

- `sp_arsenal_bird` は `By Blood Alone` を必須にしている
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:652>))
- 他 project では `By Blood Alone` と `No Step Back` の両方を要求する例もある
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:827>))
- `ai_will_do` は `base` と `modifier` で重み付けでき、
  `original_tag = USA` など特定国だけを強く優先させられる
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:711>))

本MOD向け判断材料:

- 「特定国だけが着手しやすい超兵器 project」は実装可能
- ただし DLC ガードを入れるなら、`SPEC.md` 6.3 の前提どおり
  BBA 依存を明示する必要がある

##### 6. Special Project に紐付くアイコン・ローカライズ

- `sp_arsenal_bird` の表示名は `sp_arsenal_bird` キーそのまま
  ([replaced_from_special_projects_l_english.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/english/replace/replaced_from_special_projects_l_english.yml:253>),
  [replaced_from_special_projects_l_japanese.yml](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/localisation/japanese/replace/replaced_from_special_projects_l_japanese.yml:360>))
- `sp_arsenal_bird_desc` は今回確認した置換ローカライズには無い
- アイコン指定は project 側で `icon = GFX_sp_aircraft_experimentation`、
  sprite 実体は `interface/special_projects/MD_project.gfx` にある
  ([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:654>),
  [MD_project.gfx](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/interface/special_projects/MD_project.gfx:280>))
- reward 側は既定値として `name = reward_id`, `desc = reward_id_desc`,
  `icon = GFX_reward_id` を使う説明がある
  ([special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:273>),
  [special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:284>),
  [special_projects_documentation.md](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/special_projects_documentation.md:290>))

##### 本MOD向け Special Project 追加設計メモ

超大型潜水艦用は、`specialization_naval` と `sp_tag_naval_vehicles` を使う
`sp_super_submarine` 系 project にまとめる構成が素直。`project_output` の
`enable_equipments` で `super_submarine_equipment_*` variant を解放し、前提条件は
`available` 内で既存の原子力・潜水艦寄り project 完了を要求する形に寄せられる。

空中艦系は、`mothership_equipment_*` variant ごとに project を分ける方が
管理しやすい。`specialization_air` を使い、`project_tags` は
`sp_tag_avionics_aeronautics` を基本、無人機色が強いものだけ
`sp_tag_pilotless_vehicles` を使う構成なら、`sp_arsenal_bird` と同じ流れで
equipment と専用 module を同時解放できる。

#### HOI4「空母メカ」内部構造と空軍流用の制約(2026-06-28 調査)

本人発案「アーセナルバードのような母機-子機関係を HOI4 で実体運用したい」を受けた
HOI4 空母メカの内部構造調査結果。**ここで判明した知見は別MOD技術検証
(SPEC.md 2.2.5)の前提資料**として使う。

##### HOI4 空母メカの3点セット

| 部品 | 役割 | 既存実装場所 |
|------|------|--------------|
| `type = carrier` archetype | 艦種定義 | `MD_naval_units.txt` |
| `carrier_size = N` stat | 艦船(空母)が運用できる艦載機数 | `module_flight_decks_category` モジュール内、`MD_ship_modules.txt:1641` 他、コメント `#carrier_size - number of planes that can operate from the ship` |
| `carrier_capable = yes` フラグ | 「この航空機は艦載機として運用される」マーカー | 航空機 archetype 側、`cv_small_plane_airframe` 等(`MD_plane_airframes.txt:1129` 他) |

##### 航空機 archetype に流用する場合の不確定要素

- エンジン側の艦載機運用ロジックは **C++ ハードコードの可能性が高い**。
  特に「空母から発艦して空戦に参加し、空母に戻る」「空母が沈むと艦載機も損耗」等の
  挙動はゲームメカの根幹で、modder が拡張できる範囲を超える
- 航空機 equipment に `carrier_size` stat を持たせても **エンジンが認識する保証は無い**。
  認識しても「stat が記録されるだけで挙動に反映されない」可能性が現実的
- ただし、これは **MOD 範囲で実機テストすれば判定できる**。30分〜1時間規模の
  試作 MOD で白黒つく見込み

##### 取り得る代替案(エンジンが認識しない場合)

- Y案: `scripted_effect` + `on_action = monthly_pulse` 等で「母機保有国に
  子機を自動生産・配備」。実体ユニットは生まれるが、母機-子機の戦闘連携は
  HOI4 メカ的に発生しない
- Z案: 現状の「母機ステータス強化モジュール」(MD `plane_droneswarm_weapon`
  パターン)で抽象表現。本MODの「子機搭載ドローン群」モジュールは既にこれ

##### 検証 MOD のテンプレ案(別セッション用メモ)

別MOD `acm-md-experiment-air-carrier` の最小構成:

```
- common/units/equipment/
    - test_super_mothership_plane.txt
      - archetype 1個 + variant 1個、carrier_size = 3 を持たせる
    - test_drone_plane.txt
      - archetype 1個 + variant 1個、carrier_capable = yes
- common/units/equipment/modules/
    - test_plane_carrier_module.txt
      - module_flight_decks_category 相当を航空機側で機能するか確認
- descriptor.mod / acm-md-experiment-air-carrier.mod
```

HOI4 起動 → 装備デザイナーで test_super_mothership_plane を作成 →
test_drone_plane を装備可能か、生産時に紐づくか、空戦時に子機が出現するかを
ログと装備画面で確認。

##### 検証結果(2026-07-18 実機テスト完了、判定: 不可)

試作 MOD `C:/dev/acm-md-experiment-air-carrier` で HOI4 1.19 + MD β版で実機検証を実施。
上記「不確定要素」に対する確定結論:

**A. `carrier_size` を plane equipment に載せた場合の挙動**
- `common/script_enums.txt:90` の `script_enum_equipment_stat` に列挙 → syntactically 有効な stat
- 実機で HOI4 の setup.log に
  `Equipment(s) loaded 'common/units/equipment/test_super_mothership_plane.txt' #2` が出力される
  (equipment 登録は成功)
- error.log の警告は `script_enum_equipment_bonus_type` 未登録の documentation warning のみ、
  致命エラー無し
- **しかし stat が登録されるだけで、C++ ロジックが plane を carrier として認識する経路は無い**

**B. `type = carrier` は naval 専用の hardcoded enum**
- vanilla `common/units/equipment/_documentation.md` にて厳格分類:
  - Naval types: `capital_ship|submarine|screen_ship|carrier|convoy|naval_transport`
  - Air types: `air_transport|fighter|cas|interceptor|tactical_bomber|strategic_bomber|naval_bomber|missile|suicide`
- `carrier` は naval 側のみ、航空機に付与不可

**C. `carrier_capable` は UI/GUI 参照のフラグ**
- `interface/airwingdetails.gui` line 52, `airwingreorganization.gui` line 753 等で
  `carrier_capable_icon` として艦載機アイコン表示条件に使用
- 実挙動(艦載機として艦艇に stationing) は C++ 側で `type = carrier` の艦艇と carrier_size と
  `carrier_capable = yes` の 3 点セットが揃った時にのみ機能する
- plane→plane の親子関係を扱う data structure が HOI4 に存在しない = 母機-子機を
  MOD 範囲で実装する経路は無い

**帰結**
- SPEC.md 2.2.5 の判定基準表の下段(エンジン無視 → Y案 or Z案)確定
- **本MOD は Z案採用**(現状の `plane_droneswarm_weapon` 系モジュールで抽象表現継続)
- Y案(scripted_effect 疑似補充)は実体生成のみで戦闘連携無し、実装コストに見合わず却下

##### 検証途中の副次発見(本件とは独立、要注意)

1. **HOI4 1.19 launcher が `.mod` の `path=` を絶対パスで書き換える挙動**
   - ローカル MOD を配置しても、setup.log で当該 MOD の equipment/modules ファイルの
     `loaded` ログが出ない = 実際には読まれていない状態が発生する
   - 回避方法: `.mod` を `path="mod/<name>"` の相対パス書式にして、Windows の
     **readonly 属性**(`attrib +R <.mod file>`)を付与すると launcher の書き換えを防止できる
   - 検証中、この対策で試作 MOD の equipment ロード成功を確認
   - **本MOD `acm-md.mod` も同様の絶対パス書式**のため、実際にゲームに反映されているか
     setup.log で確認する必要あり(未確認、要調査)。 検証セッション時点では
     `acm_plane_modules.txt` の Module(s) loaded 行が setup.log に見当たらなかった
   - **2026-07-18 確認・対策実施済み**: 本MOD側の setup.log/game.log を
     Claude Code 側で grep したところ `acm` 関連エントリ 0件で load 未確認を確定。
     `mod/acm-md.mod` を相対パス `path="mod/acm-md"` に書き直し、`chmod 444`
     で readonly 化。本人の HOI4 再起動時に load される見込み(未検証、要確認)

2. **`air_attack` は plane equipment でも有効な stat**
   - vanilla `x_plane_airframes.txt` の `cv_fighter_equipment_0` line 441 等で使用
   - 当初 AA(対空砲)専用と誤解していたが、航空機の空対空攻撃力としても機能する

---

#### MD既存のAC機体候補アイコン素材(2026-06-28 本人発見)

本人から「諦めていた機体アイコンについてはそもそもMDが一部追加していた、
ADF-01, ADF11 などが確認できた」との発見報告。MD β版 `3374271790` の
技術ツリー用アイコンとして以下の素材が確認できた。

| MD既存ファイル | 場所 | AC作品で活用候補 |
|----------------|------|------------------|
| `Falken-A.dds` | `gfx/interface/technologies/SWE/AIR/` | ADF-01 FALKEN(設定上は Saab Falken だがファイル名一致) |
| `Falken-B.dds` | 同上 | ADF-01 上位tier 候補 |
| `Falken-C.dds` | 同上 | ADF-01 最上位tier 候補 |
| `Fenrir.dds` | `gfx/interface/technologies/ENG/AIR/` | XFA-33 Fenrir |
| `Raven.dds` | `gfx/interface/technologies/ENG/LAND/` | ADF-11F Raven(設定上は別ものだがファイル名一致、要設定整合) |

##### 含意

- 本MOD方針「機体本体追加は方針外」は維持しつつ、**将来 mothership_equipment
  流用パターンで超大型空中艦アーキタイプを追加する場合**、機体本体アイコンを
  新規作成する必要が無い(MD既存流用で完結する可能性が高い)
- 種別②架空機の本MOD実装方針は変更しないが、「機体本体実装に踏み込むなら
  素材コストが低い」事実は記録しておく
- 将来追加検討時は、これらのファイルを `texturefile` 直接参照
  (SPEC.md 3.8「外部MODアイコン流用テクニック」)で活用する

##### 確認方法(本MOD実装時)

```
find "C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/gfx" \
  -type f -iname "<候補名>*.dds"
```

ADF / FALKEN / ADFX / X-02 / Wyvern / Nosferatu / Fenrir / Raven 等の名前で
検索すると、MD技術ツリー用に確保されているファイルがリストアップされる。
ファイル名がAC機体と一致しても、in-universe 設定が違う場合(例: SAAB Falken)
があるので、本MODが流用する際は **見た目の流用** のみとし、設定考証は
DATABASE.md の典拠URLに準拠する。

---

#### 独自モジュールカテゴリを HOI4 に導入する困難(2026-07-18 判明)

##### 事象

本MODで独自カテゴリ `acm_plane_special_weapons` を宣言し、TLS 2件を
このカテゴリで定義したところ、setup.log の Module(s) loaded 行が
`#1`(PLSL のみ)になり、**TLS 2件は無音でドロップ**された(warning 無し)。

##### 原因

1. HOI4 は「モジュール定義側で category = X と書くだけ」では新カテゴリを
   登録しない。実際にはそのカテゴリを `allowed_module_categories` で受け入れる
   archetype(または明示的なカテゴリ定義ファイル)が事前に存在する必要がある
2. 本MODは `acm-md/common/units/equipment/acm_plane_airframes.txt` で
   通常戦闘機 archetype に `acm_plane_special_weapons` を追加していたが、
   **MD descriptor.mod が `replace_path = "common/units/equipment"` を宣言している**
   ため、本MOD側のこのファイルは HOI4 ロード時に完全消滅
3. 結果: カテゴリを受け入れる archetype が存在しない → TLS 2件は「宛先の無い
   モジュール」として黙って捨てられる

##### 対策

- **独自カテゴリを持ち込みたい場合、MD側 airframes を丸ごとコピー+上書き
  が必要**(replace_path 制約の突破)。ただし MD 更新追従の管理コストが極めて
  大きく、本MOD方針外
- 現実的解: **既存 MD カテゴリ(`plane_fighter_weapons` 等)で登録**する。
  UIグループ分けの見た目は諦め、通常兵装と同じ選択肢として表示される
- 本MOD で TLS/HPM/機載レールガン/子機ドローン群 は `plane_fighter_weapons` に
  統一(2026-07-18 修正)

##### 教訓

- **setup.log の `Module(s) loaded 'X.txt' #N` の `#N` はモジュール数**。
  期待した個数と異なる場合、書式エラーではなく **未定義カテゴリで無視されている**
  可能性を疑う
- HOI4 は無効モジュールを警告なく無視するため、error.log だけでは検出できない
- 「新カテゴリ導入」は airframes 側のスロット拡張とセットでないと機能しない。
  MD の replace_path 制約下では大幅な設計調整が必要

---

#### MOD配置とランチャー認識の落とし穴(2026-06-28 確定)

P5 PLSL 試作の動作確認時にハマった項目を恒久記録する。本MOD以外のサブMOD
配置時にも同じ罠を踏み得る。

##### (a) HOI4 ドキュメントパスは OneDrive 同期 + 日本語ロケールで化ける

Windows 日本語版 + OneDrive 同期環境では、HOI4 の MOD/設定保存先は
**`C:\Users\<user>\Documents\Paradox Interactive\...`** ではなく
**`C:\Users\<user>\OneDrive\ドキュメント\Paradox Interactive\...`** になる。

- 判定法: 当該ディレクトリに `logs/`、`launcher-v2.sqlite`、`dlc_load.json`、
  `settings.txt`、既存 `mod/ugc_*.mod` が並んでいる側が正(本物)。
  もう一方には `mod/` だけが残った空殻が落ちていることがあり、誤コピーで
  「追加されていない」現象になる。
- 教訓: 新規サブMOD を配置する前に必ず `logs/` の有無で本物の場所を確認。

##### (b) `.mod` ファイルは MD既存サブMOD と同書式に揃える

本MODの初回配置で descriptor を独自書式(行順入れ替え・スペースインデント・
name に括弧含む)にしたためランチャー認識に失敗した可能性が高い。
動作している `mod/MD_TSF_Submod.mod` の書式を完全コピーすると認識した。

- 行順: `name` → `version` → `tags` → `supported_version` → `path`(`.mod` のみ)
  → `dependencies`
- インデント: **タブ**(スペース4ではない)
- `mod/<id>.mod` と `mod/<id>/descriptor.mod` の両方が必要。中身は
  `path=` 行の有無だけが違う(`.mod` 側のみ持つ)

##### (c) ランチャーは `.mod` の `path` を絶対パスに自動書き換える

ユーザーがランチャーで MOD を認識・有効化すると、`mod/<id>.mod` の `path` 行が
`path="mod/<id>"`(相対)から `path="C:/Users/.../mod/<id>"`(絶対)に自動で
書き換わる。これは認識成功の証拠であり、エラーではない。

`mod/<id>/descriptor.mod`(MODフォルダ内側)には `path` 行を書かない。これも
MD_TSF_Submod 慣習に合わせる。

##### (d) `dlc_load.json` の有効化は本人がランチャー UI 経由で行う

`dlc_load.json` を直接編集してもランチャー起動時にプレイセット設定で上書き
されてしまうため意味が無い。**MOD の有効化はランチャー UI で MOD タブの
「ローカル」リストから本人が操作する**のが唯一の確実な方法。

エージェント側がやるべきは、`mod/<id>.mod` と `mod/<id>/descriptor.mod` を
正しい場所に正しい書式で置くまで。

---

#### 外部MODアイコンの流用テクニック(2026-06-28 確定)

本MODの **アイコン素材方針は「MD既存アイコン流用を最優先」**(SPEC.md 3.8)。
新規 `.dds` を作るよりも、MD既存の `.dds` を流用する方が安全・軽量・
意図明示の点で有利。具体的な実装テクニックは以下:

##### .gfx の texturefile に MD既存パスを直接書く

HOI4 はテクスチャ探索パスを **自MOD → 依存MOD(MD) → ベースゲーム** の順で
解決する。よって本MODは `.dds` 実体を持たず、`.gfx` の `texturefile` だけを
MD既存のパスに向ければ、HOI4 が自動的に MD MOD フォルダから .dds を引いてくる。

実例(PLSL、2026-06-28 実装):

```
spriteTypes = {
    spriteType = {
        name = "GFX_EMI_acm_pulse_laser_1"
        texturefile = "gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds"
    }
}
```

これは MD既存の `GFX_EMI_spec_countermeasures_4`(`plane_countermeasures`
カテゴリ最上位 tier)のテクスチャを流用したもの。本MOD側に `.dds` 実体は無いが、
HOI4 ロード時に MD MOD フォルダの該当ファイルが解決される。

##### モジュール系統ごとの流用先(2026-06-28 本人指定、随時拡張)

| 系統 | 流用先 .dds | 由来 MD sprite |
|------|------------|-----------------|
| 指向性エネルギー兵器(PLSL / TLS / HPM / 機載レールガン 等) | `gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds` | `GFX_EMI_spec_countermeasures_4` |

他系統(電子戦 / ドローン / ミサイル / AI支援 / 艦載兵装 等)は実装時に
本人と相談して都度確定する。

この方式の利点:

- 本MODのリポジトリサイズが小さい
- MD 側でアイコン .dds が更新されても本MODが追従不要(同じパスが
  存在する限り自動反映される)
- 「流用してます」と意図が `.gfx` 1行で明示される
- ライセンス上もファイルコピーを伴わないので安全

注意点:

- MD 側でファイルパス自体が変わると参照切れ(その場合は本MODの `.gfx` を
  更新)
- MD β版と Steam 正式版で `texturefile` のパスが異なる可能性は要確認
  (現時点では同一前提で運用)
- 自MOD名と同名の sprite 名(例: `GFX_EMI_<acm_module_id>`)を必ず使い、
  既存sprite名と衝突させないこと

##### 流用元アイコンの探し方

1. MD既存モジュールの一覧から類似機能・類似カテゴリのものを選ぶ
2. その `GFX_EMI_<id>` 登録箇所を `interface/plane_modules_icons.gfx` 等で
   `grep` して `texturefile` を抜き出す
3. 本MOD `.gfx` でその texturefile を別 sprite 名(`GFX_EMI_acm_<id>`)で
   再登録する

例:

```
$ grep -A1 'weap_multi_gun_3' interface/plane_modules_icons.gfx
        name = "GFX_EMI_weap_multi_gun_3"
        texturefile = "gfx/interface/technologies/0_modules/plane_builder/weapons/weapon_multi_cannon_2.dds"
```

##### 流用が困難な場合の方針

MD既存に視覚的に近いアイコンが無い場合のみ、自MOD内に `.dds` を新規追加する。
その場合は P3-B 調査のサイズ・圧縮形式に従う(`76x42` RGBA32 が plane module の
標準サイズ)。

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
