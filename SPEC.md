# SPEC.md — 仕様

本ドキュメントは、ACモジュールMOD (ACM-MD) の仕様を記述する。
PLAN.md(やりたいこと)を、実装可能な形に翻訳したもの。

仕様は段階的に確定する。
- 確定済みの項目は「**確定**」と明記
- 未確定の項目は「**未確定 / 要決定**」と明記し、決定のための前提条件を記す
- MDの実コード精査(Claude Code/Codex 側で実施)を経て更新される項目は
  「**要調査**」と明記

---

## 1. MODの全体構成

### 1.1 配置形態(確定)

Millennium Dawn のサブMODとして実装する。
MD本体を改変するのではなく、上書き/追加で動作させる。

### 1.2 ファイル構成(一部確定)

HOI4 mod の標準ディレクトリ構造に従う:

- `common/units/equipment/modules/` — 新規モジュール定義
- `common/special_projects/projects/` — 新規 Special Project(該当する場合)
- `common/technologies/` — 解放用技術(該当する場合)
- `gfx/interface/` — モジュールアイコン
- `interface/` — gfx登録
- `localisation/english/` — 英語ローカライゼーション
- `localisation/japanese/` — 日本語ローカライゼーション(対応する場合)

2026-06-28 時点で、Steam β版 `3374271790` の実コード確認により、
アーキタイプ本体は `common/units/equipment/`、モジュール定義は
`common/units/equipment/modules/` に置かれていることを確認済み。
細部の命名規則と登録先は 3.7 / 3.8 / 3.9 を参照。

### 1.3 命名規則(一部確定)

- モジュールID接頭辞: `acm_` を予定(ACM-MDの略)
- 既存MDモジュールと衝突しないことを `grep` で確認する
- 例: `acm_railgun_main_battery`, `acm_railgun_secondary`, `acm_sluav_bay`
- Steam β版の既存実装では、モジュール本体は `abbreviation`, `category`,
  `sfx`, `xp_cost`, `add_stats`, `build_cost_resources` を基本に定義され、
  `parent`, `can_convert_from`, `allow_mission_type`, `multiply_stats` などの
  任意フィールドを追加するパターンが確認済み

---

## 2. 追加するもの(カテゴリ別の枠組み)

### 2.1 モジュール追加(MODのメイン)(確定方針)

既存MDアーキタイプのスロットに、新カテゴリのモジュールとして追加する。

対象アーキタイプ(暫定):

| 系統 | アーキタイプ(例) | 想定する追加モジュール例 |
|------|---------------------|-----------------------------|
| 艦船 | battleship, cruiser, missile_submarine 等 | 電磁レールガン砲塔、大型レールキャノン、SLUAV発射ベイ、発展型VLS |
| 航空機 | small/medium/large_plane_airframe, mothership_equipment | COFFIN系アビオニクス、APS、TLS、その他超兵器級兵装 |
| ヘリ | (helicopter modules) | 未定 |
| 戦車 | (tank chassis modules) | 未定(AC作品次第) |

具体的な兵器は DATABASE.md を基に選定する。

### 2.2 Special Projects + アーキタイプ追加(余裕があれば)(確定方針)

1点物の超兵器のみ、MDの既存パターン(`sp_arsenal_bird` + `mothership_equipment`)に倣って実装。
ただしアイコン制約のため、追加するか否かは作品選定後に判断する。

### 2.3 解放方法(確定)

入手条件は「研究」で解放する。
- 既存技術ツリーに紐付けるか、新規技術を追加するかは要決定
- Special Projects経由のものは前提研究を設定可能

### 2.4 性能チューニング方針(確定)

- 原作の特徴は再現するが、コスト・製造難易度・必要技術で抑制する
- MD既存装備と並んで「選択肢になる」レベルを目指す
- 数値の具体的バランスは DATABASE.md と MD実装の対比後に決定

---

## 3. モジュール定義の技術仕様(一部確定)

2026-06-28 時点で、Steam β版 `3374271790` の `MD_plane_modules.txt`,
`MD_ship_modules.txt`, `MD_plane_airframes.txt`, `MD_mtg_ships.txt` を精査し、
以下を確定した。

### 3.1 モジュール定義の基本書式(確定)

- 基本フィールド:
  `abbreviation`, `category`, `sfx`, `xp_cost`, `add_stats`, `build_cost_resources`
- 任意フィールド:
  `add_equipment_type`, `allow_mission_type`, `multiply_stats`, `parent`,
  `can_convert_from`, `manpower`, `add_average_stats`, `dismantle_cost_ic`,
  `critical_parts`
- 上位tierは `parent` で継承し、`can_convert_from` で改修元モジュールと
  `convert_cost_ic` を指定するパターンが一般的

### 3.2 新カテゴリを既存アーキタイプで受ける方法(確定)

- アーキタイプ側の `module_slots` にある `allowed_module_categories` へ
  対象カテゴリを追加する
- 追加後も `module_count_limit` による上限設定を別途確認する必要がある
- `module_count_limit` は次の2形式が確認済み:
  - `module_count_limit = { category = <category> count < N }`
  - `module_count_limit = { module = <module_id> count < N }`

### 3.3 性能修正値の書式と利用可能キー(確定)

- 定数加算は `add_stats`
- 割合補正は `multiply_stats`
- 2026-06-28 時点で実際に使用確認できた主なキー:
  - 航空機系: `air_superiority`, `air_attack`, `air_agility`, `air_defence`,
    `air_ground_attack`, `air_bombing`, `air_range`, `naval_strike_attack`,
    `naval_strike_targetting`, `thrust`, `maximum_speed`, `weight`,
    `night_penalty`, `reliability`, `fuel_consumption`
  - 艦船系: `naval_speed`, `naval_range`, `surface_detection`,
    `surface_visibility`, `sub_detection`, `sub_visibility`, `sub_attack`,
    `anti_air_attack`, `lg_attack`, `hg_attack`, `hg_armor_piercing`,
    `torpedo_attack`, `carrier_size`, `carrier_surface_detection`,
    `carrier_sub_detection`, `mines_planting`, `mines_sweeping`,
    `max_strength`, `max_organisation`, `armor_value`, `supply_consumption`

### 3.4 `build_cost_resources` の有効資源名(確定)

モジュール定義で実使用を確認した資源:

- `aluminium`
- `chromium`
- `composites`
- `microchips`
- `steel`
- `tungsten`

### 3.5 `upgrades` ブロックの仕組み(確定)

- `upgrades` はアーキタイプ/variant 側で利用可能な upgrade 系列を列挙する
- 確認できた代表例:
  - 航空機: `plane_bba_radar_upgrade`, `plane_bba_engine_upgrade`,
    `plane_bba_bomb_upgrade`, `plane_bba_turret_upgrade`
  - 艦船: `ship_CM_upgrade`, `ship_ground_support`, `ship_VLS_upgrade`,
    `ship_gun_upgrade`, `ship_armor_upgrade`, `carrier_engine_upgrade`,
    `carrier_displacement_upgrade`, `ship_reliability_upgrade`,
    `sub_displacement`, `sub_sonar_upgrade`, `sub_stealth_upgrade`,
    `sub_engine_upgrade`, `sub_torpedo_upgrade`

### 3.6 継続して要調査の項目

- MD独自の改変点のうち、上記以外の周辺仕様

### 3.7 ローカライズキー命名規則(確定)

- モジュール本体・equipment・技術は、基本的に `ID` が表示名、
  `ID_desc` が説明文
- モジュールカテゴリ表示名は `EQ_MOD_CAT_<category>_TITLE`
- Arsenal Bird 系のように、カテゴリ名よりスロット名
  (`EQ_MOD_SLOT_<slot>_TITLE`) で見せている実装もある
- Special Project 名は `project_id` そのままでローカライズされる
- 詳細な実例と参照先は `KNOWLEDGE.md`「ローカライズキー命名規則」参照

### 3.8 アイコン .dds 仕様 + .gfx 登録(確定)

- モジュール・Special Project・MIO では、アイコン規格が用途別に分かれている
  (例: plane module 主流は `76x42`、Special Project 主流は `206x106`,
  MIO 主流は `48x48`)
- plane module 系は `interface/plane_modules_icons.gfx` の
  `GFX_EMI_<module_id>`、airframe 中サイズ画像は `GFX_<equipment_id>_medium`
  で登録されている
- Special Project は `icon = GFX_sp_*` を project 定義側で明示し、
  `interface/special_projects/MD_project.gfx` で sprite 登録する
- アーキタイプ本体では `picture = archetype_*` と `sprite = <short_name>` の
  2系統を併用している
- 本MOD側は `plane_modules` / `airframes_medium` / `special_projects` など
  **用途別に別 `.gfx` を追加**して分割管理する
- **アイコン素材方針(2026-06-28 本人確定)**: **MD既存アイコンの流用を最優先**。
  本MODでは新規 `.dds` 実体を持たず、自MODの `.gfx` でMD既存の `texturefile`
  パスを直接参照する形を基本とする。HOI4 のテクスチャ探索パスは
  自MOD → MD → ベースゲーム の順で解決されるため、MOD依存があれば動作する。
- **モジュール系統ごとの流用先(2026-06-28 本人指定、随時拡張)**:
  - **指向性エネルギー兵器系**(PLSL / TLS / HPM / 機載レールガン 等) →
    `gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds`
    (MD既存 `GFX_EMI_spec_countermeasures_4` 由来。ハイテク兵装らしい見た目で
    レーザ・電磁兵器系のアイコンとして統一流用)
  - 他系統(電子戦 / ドローン / ミサイル / AI支援 / 艦載兵装 等)は実装時に
    本人と相談して都度確定
- 実例(PLSL): `acm-md/interface/acm_plane_modules.gfx` で
  `GFX_EMI_acm_pulse_laser_1` を定義し、`texturefile` に MD既存
  `gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds`
  を参照
- 流用が困難な場合(MD既存に視覚的に近いアイコンが無い場合)に限り、
  新規 `.dds` を自MOD内に追加する
- 詳細なサイズ・ヘッダ情報・実例は `KNOWLEDGE.md`
  「アイコン .dds 仕様 + .gfx 登録」参照

### 3.9 Special Projects 新規追加手順(確定)

- `sp_arsenal_bird` 実装で、project 定義は `allowed` / `specialization` /
  `icon` / `project_tags` / `available` / `project_output` / `ai_will_do` などを
  組み合わせて作ることを確認した
- 装備 variant 解放は `enable_equipments`、個別 module 解放は
  `enable_equipment_modules` を使う
- 本MODの超大型潜水艦は `specialization_naval` + `sp_tag_naval_vehicles`、
  空中艦系 variant は `specialization_air` + 既存 air系 tag 再利用を基本にする
- 新規 specialization は画像・facility model 連動まで増えるため、
  まずは既存 specialization / generic reward の再利用を優先する

---

## 4. 対象作品と兵器の選定

### 4.1 対象作品(確定、10作品)

発表順:
- AC2 (Ace Combat 2)
- AC04 (Shattered Skies)
- AC5 (The Unsung War)
- ACZ (Zero: The Belkan War)
- ACX (X: Skies of Deception)
- AC6 (Fires of Liberation)
- AC Joint Assault
- AH (Assault Horizon)
- AC7 (Skies Unknown)
- AC Infinity (2018年3月サービス終了済み)

**AC3 Electrosphere は2026-06-27にスコープ外と決定**(世界観・雰囲気が
シリーズの他作品と大きく異なるため。本人判断)。
DATABASE.mdに既に作成済みのAC3関連エントリ(超兵器6件)は削除済み。

### 4.2 本命作品(未決定)

上記11作品から、深掘りは1作品に絞る方針。
DATABASE.md の超兵器・架空艦セクション完成後に、
素材の豊富さとMD既存実装との接続性から本命作品を選定する。

### 4.3 第一段階で実装する兵器(2026-06-28 一部確定)

DATABASE.md の判定結果を踏まえ、最小スコープで1〜2モジュールから着手。
最初の実装対象は以下の条件を満たすものを優先する:

- HOI4実装判定が `○ モジュール化可`
- MD既存実装と機能が被らない
- アイコンが既存流用または最小限の新規作成で済む
- 効果が分かりやすく、動作確認しやすい

#### 通常戦闘機(`small_plane_airframe` 系列)へのモジュール受入可否表

`MD_plane_airframes.txt:53-174` の `small_plane_airframe` スロット構成を
2026-06-28 に直接確認した結果、以下のとおり:

| 種別④モジュール | カテゴリ | 受入スロット | 実装容易性 |
|------------------|----------|--------------|------------|
| PLSL | plane_multipurpose_gun | `fixed_gun_slot` | 完了(v0.1.0) |
| COFFIN コックピット | plane_avionics | `avionics_type_slot` | モジュール追加のみ |
| 機載AI支援 | plane_avionics | 同上 | モジュール追加のみ |
| ベルカ系ECM | plane_countermeasures | `special_slot_type_1` | モジュール追加のみ |
| 攻撃型ECM "Cocytus" | plane_countermeasures | 同上 | モジュール追加のみ |
| MPBM | plane_fighter_weapons | `fixed_main_weapon_slot` / `auxiliary` | モジュール追加のみ |
| 多目的全方位ミサイル | plane_fighter_weapons | 同上 | モジュール追加のみ |
| LSWM | plane_fighter_weapons または plane_heavy_nav_weapons | 同上 | モジュール追加のみ |
| UAVネットワーク制御 | plane_drone_systems | `fixed_main_weapon_slot` | モジュール追加のみ |
| **TLS** | plane_heavy_special_design_arsenal | 通常戦闘機に該当スロット無し | **要 Special Project 経由スロット解放** |
| **HPM** | 同上 | 同上 | **要 Special Project 経由スロット解放** |
| **機載レールガン** | 同上 | 同上 | **要 Special Project 経由スロット解放** |
| **子機搭載ドローン群** | plane_droneswarm_weapon | 同上 | **要 Special Project 経由スロット解放** |

#### 実装の順序方針(2026-06-28 確定)

- **第1群「モジュール追加のみで通常戦闘機に乗るもの」** を優先実装。
  PLSL(完了)+ 8件の合計9件は本MOD単体で完結し、MD アーキタイプを
  上書きしない安全な実装になる
- **第2群「Special Project 経由でスロット解放が必要なもの」**
  (TLS / HPM / 機載レールガン / 子機搭載ドローン群)は、`sp_arsenal_bird`
  パターンに倣う Special Project を追加して、専用スロットを解放する設計に
  する。MD descriptor.mod が `common/units/equipment` を replace_path 宣言
  しているため、アーキタイプ直接上書きは衝突リスクが高く、Special Project 経由が
  より安全
- **第3群「艦載モジュール5件」**(SRC-03a超大型レールキャノン / 補助レールガン /
  バースト弾頭ミサイル / 潜水艦用艦上滑走路 / SLUAV発射ベイ)は、新規
  「超大型潜水艦」アーキタイプ + Special Project とセットで実装する

---

## 5. スコープ外(確定)

以下は本MODでは扱わない:

- AC作品の通常兵装(4AAM、6AAM、QAAM、SAAM、マルチロックオン系)
- 固定設置型超兵器(ストーンヘンジ、エクスキャリバー、メガリス等)の
  モジュール化
- 量産戦闘機の本体追加(MDの既存実装で十分)
- 機体・艦本体のアイコン新規作成
- 架空車両・実在車両・実在艦のDB化
- 陣営性の固有化(オーシア/エルジア専用モジュール等)
- AC作品由来の通常機の追加(機体本体は追加せず、モジュールで個性化)
- AC3 Electrosphere、AC8、AHL、AC Xi、AC Northern Wings、AC Advance の
  DB化(対象外作品。AC3は2026-06-27にスコープ外決定)

---

## 6. 互換性

### 6.1 MD本体との互換性(確定)

- Millennium Dawn の最新版に追従する
- GitHub上の開発版とSteam版で差分が出る可能性があるため、
  実装時はプレイ環境(Steam版)を基準にする

### 6.2 他のサブMODとの互換性(未確定)

- 同じファイルに上書きを行う他のサブMODと併用した場合、
  後勝ちで挙動が変わる可能性がある
- 既知の主要サブMODとの干渉点は、実装後に KNOWLEDGE.md に
  記録していく

### 6.3 HOI4本体DLCとの依存(一部確定)

- MD自体が NSB / MtG / BBA / By Blood Alone 等のDLCに依存している
- 本MODはMDの依存に追従するため、独自のDLC依存は持たない予定
- Special Projects を使う場合は By Blood Alone が前提
  (`sp_arsenal_bird` の `allowed = { has_dlc = "By Blood Alone" }` を
  Steam β版 `air_projects.txt` で確認済み)

---

## 7. 開発体制と作業分担(確定)

| 作業 | 担当環境 |
|------|----------|
| 戦略立案、SPEC更新、AC設定調査 | claude.ai(本会話) |
| DATABASE.md 作成、MDコード精査 | Claude Code または Codex |
| モジュール定義の実装 | Claude Code または Codex |
| アイコン配置・登録 | Claude Code または Codex |
| 動作確認(ゲーム起動) | 開発者本人 |

Claude Code と Codex は同じプロジェクトルール(CLAUDE.md / AGENTS.md)に従う。
両者の同時並行作業は禁止する(コンフリクト防止)。

---

## 8. 完成判定(初期マイルストーン)

ACM-MD v0.1.0(最小動作版)の完成条件:

- DATABASE.md が完成し、本命作品が選定されている
- 最初の1〜2モジュールが MDモジュールスロットに追加できている
- ゲーム内のアーキタイプ画面で当該モジュールが選択可能になっている
- 性能修正値がゲーム内で正しく適用されている
- KNOWLEDGE.md に MDモジュール定義の知見が蓄積されている

v0.1.0 完成後の拡張は別途 PLAN/SPEC を更新する。
