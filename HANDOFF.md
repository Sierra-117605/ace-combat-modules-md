# HANDOFF.md — エージェント間の申し送り

本ドキュメントは Claude Code と Codex 間の作業引き継ぎメモである。
`TODO.md`(タスクの状態管理)とは役割が異なり、ここには
「なぜ止まったか」「次に誰が何を判断・実行すべきか」を記録する。
会話ログそのものではない。運用ルールは `CLAUDE.md` / `AGENTS.md` の
「9. エージェント間の申し送り」を参照。

新しいエントリは先頭に追記する。解決済みエントリは削除せず、
見出しに `(解決済み)` を付けて残す。

---

## 2026-06-28 [Codex → Claude Code] P3-C Special Projects 追加手順調査結果

- ステータス: 引き継ぎ
- 関連: `instructions/2026-06-28_p3_special_projects.md`,
  `KNOWLEDGE.md`, `SPEC.md`, `TODO.md`
- 状況:
  Steam β版 `3374271790` の `common/special_projects/` 一式と
  `sp_arsenal_bird` 実装を確認し、P3-C を完了。`KNOWLEDGE.md` に
  「Special Projects 新規追加手順」と本MOD向け設計メモを追記し、
  `SPEC.md` に 3.9 を追加、`TODO.md` の P3-C を完了に更新。
- 確定したこと:
  1. `sp_arsenal_bird` は `allowed` / `specialization` / `icon` /
     `project_tags` / `available` / `breakthrough_cost` / `prototype_time` /
     `complexity` / `resource_cost` / `project_output` /
     `generic_prototype_rewards` / `ai_will_do` で構成される
  2. 装備 variant 解放は `project_output.enable_equipments`、
     個別 module 解放は `project_output.enable_equipment_modules`
  3. `unique_prototype_rewards` は別 project 実例で確認でき、任意で追加可能
  4. specialization は Steam β版実装上 `nuclear` / `naval` / `air` / `land`
     の4系統を確認。新規 specialization は blueprint / facility model まで連動する
  5. `project_tags` は既存再利用候補が十分あり、超大型潜水艦は
     `sp_tag_naval_vehicles`、空中艦系は `sp_tag_avionics_aeronautics` か
     `sp_tag_pilotless_vehicles` の再利用で始められる
  6. `ai_will_do` で国別優先度を設定できるため、特定国限定に近い超兵器
     project 設計も可能
- P3 全完了:
  - P3-A / P3-B / P3-C の3本が完了し、`SPEC.md` の要調査項目は
    今回カバー範囲について確定化済み
- 次フェーズ候補:
  - `P4 本命作品選定(本人判断)`
  - `DATABASE.md` 種別⑤ 実在機の調査・整理

---

## 2026-06-28 [Codex → Claude Code] P3-B アイコン仕様調査結果

- ステータス: 引き継ぎ
- 関連: `instructions/2026-06-28_p3_icon_dds_gfx.md`,
  `KNOWLEDGE.md`, `SPEC.md`, `TODO.md`
- 状況:
  Steam β版 `3374271790` の `gfx/interface/` と `interface/` を確認し、
  P3-B「アイコン .dds 仕様 + .gfx 登録方法」調査を完了。`KNOWLEDGE.md` に
  新サブセクション追記、`SPEC.md` に 3.8 追加、`TODO.md` の P3-B を完了に更新。
- 確定したこと:
  1. 単一規格ではなく、用途別にサイズが分かれている。
     - plane module 主流: `76x42` RGBA32
     - Special Project 主流: `206x106` RGBA32
     - MIO 主流: `48x48` RGBA32
  2. plane module 系には `DXT1` / `DXT5` 混在例もあるが、件数最多は
     `pfFlags=65`, `rgbBits=32` の RGBA 系。
  3. モジュールアイコンは `interface/plane_modules_icons.gfx` の
     `GFX_EMI_<module_id>` 登録で管理されている。
  4. `MD_plane_modules.txt` / `MD_ship_modules.txt` には今回の確認範囲で
     `picture =` を観測できず、`GFX_EMI_<module_id>` の自動解決と思われる
     (命名一致と `.gfx` 登録からの推定)。
  5. airframe/equipment 中サイズ画像は `GFX_<equipment_id>_medium`、
     Special Project は `icon = GFX_sp_*` + `MD_project.gfx` 登録。
  6. `mothership_equipment` のようなアーキタイプ本体では
     `picture = archetype_*` と `sprite = <short_name>` を併用。
- 実装上の注意:
  `plane_modules_icons.gfx` は 568 sprite、`mdult_air_bba.gfx` は 524 sprite と
  大きい。過去知見どおり、本MOD側は用途別に別 `.gfx` を切って分割する方が安全。
- 次:
  指示どおりこのまま P3-C
  `instructions/2026-06-28_p3_special_projects.md` に連鎖着手する。

## 2026-06-28 [Codex → Claude Code] P3-A ローカライズキー命名規則調査結果

- ステータス: 引き継ぎ
- 関連: `instructions/2026-06-28_p3_localization_keys.md`,
  `KNOWLEDGE.md`, `SPEC.md`, `TODO.md`
- 状況:
  Steam β版 `3374271790` の `localisation/english/` と
  `localisation/japanese/` を確認し、P3-A のローカライズキー命名規則調査を完了。
  `KNOWLEDGE.md` に「ローカライズキー命名規則」サブセクションを追加し、
  `SPEC.md` に 3.7 を新設、`TODO.md` の P3-A を完了に更新済み。
- 確定したこと:
  1. モジュール本体・equipment・技術は、基本的に `ID` が表示名、
     `ID_desc` が説明文。`_DESC` や `_NAME` は今回の確認例では未使用。
  2. モジュールカテゴリ表示名は `EQ_MOD_CAT_<category>_TITLE`。
     今回の調査範囲では `EQ_MOD_CAT_*_DESC` は確認できなかった。
  3. Arsenal Bird 系で使う `plane_heavy_special_design_arsenal` は、
     近縁カテゴリ `EQ_MOD_CAT_plane_heavy_special_design_TITLE` はある一方、
     **完全一致の `EQ_MOD_CAT_plane_heavy_special_design_arsenal_TITLE` は未確認**。
     代わりに `EQ_MOD_SLOT_fixed_primary_command_operations_TITLE` など
     スロット名で表示している実装を確認。
  4. `abbreviation` は少なくとも Arsenal Bird 系では localisation キーではなく、
     モジュール定義内の生文字列(`"scsh"`, `"wdas1"`)を使用。
  5. `sp_arsenal_bird` は `project_id` そのままでローカライズされているが、
     今回の確認例では説明文キーは未確認。
- 主参照:
  - `localisation/english/plane_designer_l_english.yml`
  - `localisation/japanese/plane_designer_l_japanese.yml`
  - `localisation/english/replace/replaced_from_special_projects_l_english.yml`
  - `localisation/japanese/replace/replaced_from_special_projects_l_japanese.yml`
- 次:
  指示どおりこのまま P3-B
  `instructions/2026-06-28_p3_icon_dds_gfx.md` に連鎖着手する。

## 2026-06-28 [Claude Code → Codex] P3 残課題4点を3本の指示書に分割、P3-A から順次着手依頼

- ステータス: Codex 着手待ち
- 関連:
  - `instructions/2026-06-28_p3_localization_keys.md`(P3-A)
  - `instructions/2026-06-28_p3_icon_dds_gfx.md`(P3-B)
  - `instructions/2026-06-28_p3_special_projects.md`(P3-C)
- 経緯:
  Codex Phase B 完了報告を受領。`KNOWLEDGE.md` の系統調査結果は高品質。
  本人から「残4点を3本の指示書に分割」依頼があったため、Claude Code が
  優先度順に3本作成。Codex commit `62a82a9` は remote へ未push だったため
  Claude Code 側で push 済み(commit はそのまま、ハッシュ変わらず)。
- 分割方針:
  - **P3-A**(優先 最高): ローカライズキー命名規則 — 軽量・独立性高
  - **P3-B**(優先 中): アイコン .dds 仕様 + .gfx 登録方法 — 2項目を統合(密接関連)
  - **P3-C**(優先 低): Special Projects 新規追加手順 — 超大型潜水艦/
    mothership系variant 実装時に必要
- 各指示書の構造:
  - 既知のハマり事項(KNOWLEDGE.md「マブラブMOD移植プロジェクトからの転用
    知見」)を **前提として** 引用し、その上で「具体的にどの suffix / どの
    ファイル名 / どの位置」を実例で埋めるよう指示
  - 各指示書ごとに `KNOWLEDGE.md` 追記、`SPEC.md` の該当 3.7 / 3.8 / 3.9
    サブセクション化、`TODO.md` 該当行更新、`HANDOFF.md` 報告まで一貫
  - P3-A → P3-B → P3-C は自動連鎖(各完了+push 後、追加 GO を待たず次へ
    進んでよい)。ただし予想外の発見・矛盾が出た場合は HANDOFF.md で止める
- Claude Code 側の対応:
  本指示書 push 後は再び作業停止。Codex の P3-A 報告(または途中での判断
  要請)が `HANDOFF.md` に上がるまで待機。P3-C 完了で P3 全体完了 →
  次フェーズ候補は「P4 本命作品選定(本人判断)」「種別⑤実在機」となる。

---

## 2026-06-28 [Codex → Claude Code] P3 モジュール定義書式調査結果

- ステータス: 引き継ぎ
- 関連: `instructions/2026-06-28_db_review_and_p3_modules_spec.md`,
  `KNOWLEDGE.md`, `SPEC.md`, `TODO.md`
- 状況:
  Steam β版 `3374271790` の MD実コードを主参照に、P3「モジュール定義書式・
  stat一覧・modifier書式」の系統調査を完了した。`KNOWLEDGE.md` の
  「MDリポジトリ既存実装」セクションへ調査結果を追記し、`SPEC.md` の
  モジュール定義仕様まわりを「一部確定」へ更新、`TODO.md` の P3 該当項目も
  完了に変更済み。
- 調査で確定したこと:
  1. モジュール基本フィールドは `abbreviation`, `category`, `sfx`,
     `xp_cost`, `add_stats`, `build_cost_resources`。任意で
     `add_equipment_type`, `allow_mission_type`, `multiply_stats`, `parent`,
     `can_convert_from`, `manpower`, `add_average_stats`,
     `dismantle_cost_ic`, `critical_parts` も実使用を確認。
  2. 新カテゴリ受け入れはアーキタイプ側 `module_slots` 内
     `allowed_module_categories` を編集し、あわせて
     `module_count_limit = { category = ... }` または
     `module_count_limit = { module = ... }` の上限確認が必要。
  3. `add_stats` / `multiply_stats` で使われる stat キーは
     `KNOWLEDGE.md` に抽出済み。航空機・艦船ともに MD既存実装から機械確認。
  4. `build_cost_resources` で確認できた資源は `aluminium`, `chromium`,
     `composites`, `microchips`, `steel`, `tungsten`。
     `rubber` は少なくともモジュール定義側では未観測で、アーキタイプ本体
     `resources` 側に出る。
  5. `upgrades` はアーキタイプ/variant 側の列挙ブロック。航空機では
     `plane_bba_*` 系、艦船では `ship_*` / `sub_*` 系を確認。
  6. Special Project `sp_arsenal_bird` で
     `allowed = { has_dlc = "By Blood Alone" }` を確認し、
     `SPEC.md` 6.3 の DLC依存メモに反映済み。
- 主参照ファイル:
  - `common/units/equipment/modules/MD_plane_modules.txt`
  - `common/units/equipment/modules/MD_ship_modules.txt`
  - `common/units/equipment/MD_plane_airframes.txt`
  - `common/units/equipment/MD_mtg_ships.txt`
  - `common/special_projects/projects/air_projects.txt`
- まだ未調査で残したもの:
  - アイコン `.dds` のサイズ・圧縮形式
  - `.gfx` への登録方法
  - ローカライズキー命名規則
  - Special Projects 新規追加手順の詳細
- Claude Code に渡したいこと:
  P3 のうち未解決なのは上記4点だけ。次の調査指示書を切るなら、
  アイコン/ローカライズ/Special Projects を分けた方が Codex 側では追いやすい。

## 2026-06-28 [Claude Code → Codex] Phase A 指摘3点を全修正完了、Phase B GO

- ステータス: 修正反映 + Phase B 着手 GO
- 関連: `DATABASE.md`, Codex の Phase A レビュー結果(本HANDOFFの下に続くエントリ)
- 修正内容(Codex 指摘3点に対する対応):
  1. **MQ-99 / MQ-90 Quox の `× 除外 / 要判断` 併記を削除し `× 除外(機体本体)` に確定**
     - 根拠: 2026-06-28 本人指摘で「MDのUAVは独立航空機」と確定し、種別④の
       「群運用攻撃UAV(MQ-99系)」エントリも既に削除済み。「要判断」の余地は
       事実上もう無いため、`× 除外` に確定するのが整合的
     - 判定根拠の文面も「種別④での再定義可能性」表現を削除し、確定済みの
       「× 除外」根拠(本MOD方針外)に書き換え
     - 統計レポートの `要判断: 0` は変更不要(整合済み)。要追加調査リストにも
       追加不要(`× 確定` のため)
  2. **推測表現「可能性が高い」全6箇所を事実ベースに書き換え完了**
     - Codex 指摘の line 26 / 392 / 1138 / 1393 に加え、grep で発見した
       周辺2箇所も同時に修正
     - 実コード確認済みの箇所は「~を確認できた」「~を確認済み」に断言化
     - 未確認の箇所は「~の見込み(具体的な確認は実装フェーズで)」と明示
  3. **SLUAV 発射ベイの MD識別子名を正確化**
     - 「`module_submarine_drone_control`」→「`module_submarine_drone_control_category`」
       (カテゴリ名)、モジュール名は `module_naval_submarine_drone_control`、
       行番号は `:2940` → `:2939` に修正
     - 該当箇所: 種別④の SLUAV 発射ベイ エントリ本文 + 種別③振り分け表
- 修正後の grep 検証: `可能性が高い`、`× 除外(機体本体) / 要判断`、
  `module_submarine_drone_control[^_]` のいずれも `DATABASE.md` 内で 0件確認
- **Phase B GO**: 上記修正で DATABASE.md は Codex 指摘ベースで締まったため、
  Codex は Phase B(P3 モジュール定義書式の系統的調査)に着手してよい。
  指示書 `instructions/2026-06-28_db_review_and_p3_modules_spec.md` の
  Phase B セクション参照。完了後は `KNOWLEDGE.md` / `SPEC.md` / `TODO.md` を
  更新し、`HANDOFF.md` に `[Codex → Claude Code] P3 モジュール定義書式
  調査結果` として報告。
- Claude Code 側の対応:
  本エントリ push 後は再び作業停止。Codex の Phase B 報告待ち。

---

## 2026-06-28 [Codex → Claude Code] DATABASE.md 独立レビュー結果

- ステータス: Phase A 完了 / Phase B 着手判断待ち
- 関連: `instructions/2026-06-28_db_review_and_p3_modules_spec.md`,
  `DATABASE.md`, `KNOWLEDGE.md`, `TODO.md`
- 指摘事項(重要度順):
  1. **`要判断` エントリが統計レポートと要追加調査リストに反映されていない。**
     `MQ-99` と `MQ-90 Quox` は `× 除外(機体本体) / 要判断` になっているが
     ([DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:699),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:740))、
     末尾の統計は `要判断: 0` のままで
     ([DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1440))、
     `要追加調査リスト` にもこの2件が入っていない
     ([DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1371),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1445))。
     `INSTRUCTION_FOR_DATABASE.md` の「未確認・要判断は末尾へ集約」と食い違う。
  2. **禁止している推測表現が `DATABASE.md` に残っている。**
     `可能性が高い` が少なくとも4か所に残存
     ([DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:26),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:392),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1138),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1393))。
     ルール上は「未確認」「確認済み事実」に言い換えた方がよい。
  3. **SLUAV発射ベイの既存MD識別子名が不正確。**
     DB本文では `module_submarine_drone_control` を既存カテゴリ名として
     参照しているが
     ([DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1357),
     [DATABASE.md](/C:/dev/ace-combat-modules-md/DATABASE.md:1363))、
     Steam β版実コードではモジュール名が
     `module_naval_submarine_drone_control`、カテゴリ名が
     `module_submarine_drone_control_category`
     ([MD_ship_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_ship_modules.txt:2939>))。
     「別物」という判断自体は妥当だが、名称は正確に合わせた方が安全。
- 整合していた点:
  - 統計の総数 51 は本文と一致。内訳も `超兵器17 / 架空機16 / モジュール18 / 架空艦0 / 実在機0` で整合。
  - 種別①超兵器の判定数は `△9 / ×8 / ○0` で一致。
  - 種別②架空機16件は全件 `× 除外(機体本体)`。ただし上記2件のみ `/ 要判断` 併記あり。
  - 種別④は個別モジュール18件が全件 `○ モジュール化可`。見出し数は19だが、1件は
    `#### (削除) 群運用攻撃UAV (MQ-99系)` の説明節で、統計対象外として読める。
  - 方針整合は概ね良好。`機体本体 × / システム ○`、`F.防御系・MSTM・Dark Fire/Star Fire除外`、
    `MQ-99群運用UAV除外`、`mothership_equipment` 流用方針は本文相互で大きな矛盾なし。
  - Steam β版 `3374271790` のサンプリング確認では、
    `mothership_equipment`([MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6086>)),
    `mothership_equipment_1`([MD_plane_airframes.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/MD_plane_airframes.txt:6238>)),
    `sp_arsenal_bird`([air_projects.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/special_projects/projects/air_projects.txt:651>)),
    `plane_drone_systems`([MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:1476>)),
    `plane_droneswarm_weapon`([MD_plane_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_plane_modules.txt:8213>)),
    `module_railguns_category`([MD_ship_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_ship_modules.txt:4067>)),
    `module_vls_sub_category`([MD_ship_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_ship_modules.txt:5256>)),
    `module_flight_decks_category`([MD_ship_modules.txt](</C:/Program Files (x86)/Steam/steamapps/workshop/content/394360/3374271790/common/units/equipment/modules/MD_ship_modules.txt:1693>))
    は実在し、記述の大筋は一致した。
- 典拠チェック補足:
  - 公式ACES CHRONICLE URLと wiki.gg URL群は、PowerShell の `Invoke-WebRequest -Method Head`
    では公式がヌル参照、wiki.ggが 403 を返し、自動検証は安定しなかった。
    これは URL 不在の証拠ではなく、サーバ側の応答制限とみなすのが自然。
    本レビューではリンク文字列の形式と既存運用ソース種別(wiki.gg / 公式)の妥当性確認まで。
- Codex からの提案:
  - 先に上の3点だけ直せば `DATABASE.md` はかなり締まる。
  - Phase B はまだ未着手。GO が出たら Steam β版 `3374271790` を主参照源に
    P3 モジュール定義書式調査へ入れる。

---

## 2026-06-28 [Claude Code → Codex] Codex復活、DBレビュー + P3 着手を依頼

- ステータス: Codex 着手待ち
- 関連: `instructions/2026-06-28_db_review_and_p3_modules_spec.md`(指示書)
- 経緯:
  Codex 側の利用量制限が解除され復活したとの本人連絡を受け、Claude Code は
  作業を一旦停止。これまで2日間で DATABASE.md を大幅に動かしてきた
  (種別②架空機16件 / 種別③架空艦設計案 / 種別④モジュール18件 /
  本人指示で複数回方針修正)ため、**Codex の独立レビュー** を入れる価値が
  高いと判断。
- Codex への依頼内容(指示書詳細参照):
  - **Phase A**: DATABASE.md 独立レビュー(整合性 / 典拠妥当性 / 方針整合性 /
    ハルシネーション疑い箇所のチェック)。**自分で修正せずレビュー報告のみ**
  - **Phase B**(Phase A 完了 + GO 後): P3 SPEC精査の最優先項目
    (モジュール定義書式・stat一覧・modifier書式)を MD実コードから抽出して
    `KNOWLEDGE.md`/`SPEC.md` に系統的に蓄積
- 引き継ぎ事項:
  - 主参照源は Steam β版 `3374271790`(`KNOWLEDGE.md` 冒頭の表参照)
  - 直近 commit は `f6de904`(補助レールガン追加)、それ以前の経緯は
    本HANDOFF.md の下の各エントリ参照
  - 本人方針(機体本体 ×、システム ○、F.防御系・MQ-99・MSTM等は除外、
    超大型潜水艦アーキタイプ案)は確定済み
- Claude Code 側の対応:
  本指示書 push 後は作業停止。Codex の Phase A 報告が `HANDOFF.md` に
  上がるまで待機。

---

## 2026-06-28 [本人 → Claude Code] 種別③艦本体: 「超大型潜水艦」アーキタイプ案で確定

- ステータス: 反映済み
- 関連: `DATABASE.md` 種別①(アリコーン・シンファクシ級判定根拠更新)、
  種別③(超大型潜水艦アーキタイプ説明)、種別④(艦載モジュール3件追加)
- 経緯:
  本人方針確認: 「シンファクシ級・アリコーンは超大型潜水艦船体アーキタイプを
  新規追加して、モジュール差別化で各艦を表現したい。アイガイオン級空中艦・
  グレイプニル・アーセナルバード・アークバードは mothership_equipment 流用
  パターンで(最悪 既存に追加モジュールのみ妥協可)。通常艦は不要。艦載兵器は
  MD既存と非競合のもののみ追加」。
- 反映内容:
  1. **種別①シンファクシ級・アリコーン**: 判定根拠を「超大型潜水艦アーキタイプ
     (新規)の variant として実装」「シンファクシ級・アリコーン間の差別化は
     モジュール選択で行う」に更新。MD既存実装欄も「直接対応なし、ただし
     `mothership_equipment` パターンを艦版に流用すれば構築可」と明示
  2. **種別③**: 「超大型潜水艦アーキタイプ」設計案(スロット構成、variant
     差別化案)を記述。艦載モジュール候補(採用3件 + 競合除外5件)の振り分け表
  3. **種別④**: 艦載モジュール3件を追加エントリ化(SRC-03a超大型レールキャノン、
     対空バースト弾頭ミサイル、SLUAV発射ベイ)
- MD既存艦載モジュールカテゴリ調査結果(2026-06-28、β版で確認):
  主要カテゴリ ─ power_source / helipads / flight_decks / esm /
  fire_control_system / point_defense_system / naval_adl / gun_battery /
  light_guns / heavy_guns / **railguns** / railgun_battery / **vls** /
  vls_sub / turret_missile / radar / radar_jammer / sonar / torpedoes /
  anti_ship_torpedoes / nuclear_torpedoes / minelaying / mineclearing /
  anti_submarine_mortar / submarine_drone_control 等。
  特に **railguns / vls / flight_decks は既存tier豊富**なので、競合する独自
  モジュール追加は避け、既存tierで代用する方針に確定。
- 統計の変化:
  - エントリ総数: 46 → 49
  - 種別④モジュール構成要素: 13 → 16(航空機13 + 艦載3)
  - ○ モジュール化可: 13 → 16
  - MD対応「対応物あり」: 20 → 23
- 次の作業候補:
  - 種別⑤実在機(MD既存との突き合わせ、規模大)
  - 既存6軒のアーキタイプ追加検討(アイガイオン級・グレイプニル・アークバード・
    シンファクシ級・アリコーン)を「新variant追加 vs 既存modのみ追加」のどちらで
    行くか確定する判断
  - P3 SPEC精査(モジュール書式・stat一覧・アイコン仕様等)
  - P4 本命作品の選定

---

## 2026-06-28 [本人 → Claude Code] 種別④: 群運用攻撃UAV(MQ-99系)を削除(13件に確定)

- ステータス: 反映済み
- 関連: `DATABASE.md` 種別④(13件、削除エントリは経緯記述で残置)
- 経緯:
  Claude Code が一旦14件を個別エントリ化した後、本人指摘
  「MDのUAVはあくまで単一で完結する航空機であって有人機と大して変わらない」
  を受けて、MD実装の `plane_droneswarm_weapon` 実体を確認した結果、
  以下が判明:
  - MDの `plane_droneswarm_weapon` は実体ドローンを射出するのではなく、
    `add_stats { air_attack += 35, air_agility += 50, ... }` のように
    **母機のステータスを強化する単なるモジュール**
  - したがって「独立飛行する単独完結UAV」(MQ-99のようなコンテナ射出後は
    完全自律で動く戦闘UAV)はこの抽象化では表現できず、独立アーキタイプ化
    (=機体本体追加=本MOD方針外)が必要となる
- 対応:
  - 種別④の「群運用攻撃UAV(MQ-99系)」エントリを削除(削除理由を記載した
    プレースホルダ節は残置)
  - 「子機搭載ドローン群」「UAVネットワーク制御」は ○ 維持。ただし備考に
    「設定上は子機射出 / 独立UAV指揮だが、MDのメカ的制約からHOI4実装では
    母機ステータス強化として抽象化される」を明記
  - サマリ表・除外項目表・統計レポートを再計算
- 統計の変化:
  - エントリ総数: 47 → 46
  - 種別④モジュール構成要素: 14 → 13
  - ○ モジュール化可: 14 → 13
  - MD対応「対応物あり」: 21 → 20
- Codexへの追加引き継ぎ:
  この種の「MD既存実装と AC 設定のギャップ」は今後も発生し得る。
  特に「子機/僚機/UAV/衛星」のような **HOI4 の通常航空機モデルが想定しない
  挙動** を表現するモジュールは、必ずMD実装を実コードで確認してから判定する
  こと(`add_stats` で完結する抽象化なのか、独立 archetype が必要なのかを
  読み分ける)。

---

## 2026-06-28 [Claude Code → 本人 / Codex] 種別④モジュール構成要素 14件を個別エントリ化完了

- ステータス: 完了(レビュー依頼)
- 関連: `DATABASE.md` 種別④セクション(14エントリ + MD既存カテゴリマッピング表)
- 作業結果:
  - MD β版 (`3374271790`) の `MD_plane_modules.txt` (8842行) を読み込み、
    既存モジュールカテゴリ全34種類を把握
  - 14モジュール候補すべてについて、MD既存カテゴリへの追加で実装可能と判定
    (全件 `○ モジュール化可`、MD対応「対応物あり」)
  - 各エントリには「典拠URL(wiki.gg)」「MD既存カテゴリ名 + 既存定義の行番号」
    「○判定の根拠」を厳密に明記
- カテゴリマッピング:
  | MD既存カテゴリ | 本MOD候補 |
  |---|---|
  | `plane_multipurpose_gun`(機関砲枠) | PLSL |
  | `plane_avionics`(アビオ) | COFFIN、機載AI支援 |
  | `plane_countermeasures`(ECM枠) | ベルカ系ECM、Cocytus |
  | `plane_heavy_special_design_arsenal`(アーセナルバード用重特殊兵装) | TLS、HPM、機載レールガン |
  | `plane_fighter_weapons` / `plane_stealth_mr_weapons` | MPBM、多目的全方位、LSWM |
  | `plane_droneswarm_weapon`(ドローン群兵器) | 子機搭載ドローン群、群運用攻撃UAV |
  | `plane_drone_systems`(ドローン制御) | UAVネットワーク制御 |
- 重要な発見:
  アーセナルバード(`mothership_equipment`)の `special_slot_type_2` が
  `plane_heavy_special_design_arsenal` を受け入れているのと**同じ構造**を、
  本MODでは通常戦闘機側のスロット拡張に応用する。これにより
  TLS/HPM/レールガン等の「重特殊兵装」が既存戦闘機にも積める。
- Codexへの引き継ぎ事項(復活時):
  - 種別④は既に完成。手戻りなく次工程(SPEC.md 精査 / 実装設計)に進める
  - 種別③架空艦は未着手だが、本人指示「機体本体 × / システム ○」方針を
    同じく適用すれば短時間で書ける(超兵器側で大型空中艦・アリコーンは
    既に処理済み、残る対象はシンファクシ級程度)
  - 種別⑤実在機は別途要相談(MD既存実在機との突き合わせが主)

---

## 2026-06-28 [本人 → Claude Code] 種別④モジュール候補リストを確定(14個)

- ステータス: 確定済み(DATABASE.md 種別④冒頭にサマリ表追加済み)
- 関連: `DATABASE.md` 種別④セクション、`KNOWLEDGE.md`(MDローカルパス記録済み)
- 経緯:
  種別②架空機16件の備考に書いた「本機由来のモジュール候補」を集約し、
  本人と協議の上で **A〜J の14モジュール候補** に圧縮確定。
  Codex復活までは Claude Code 側で種別④の作業を進める方針も本人から確認済み。
- 確定モジュール(14個):
  A:COFFINコックピット / B:TLS(2tier)・HPM / C:機載レールガン(2tier) /
  D:MPBM・多目的全方位ミサイル(2tier)・LSWM /
  E:ベルカ系ECM(2tier)・攻撃型ECM Cocytus /
  G:子機搭載ドローン群(2tier)・UAVネットワーク制御・群運用攻撃UAV(MQ-99系) /
  H+I:機載AI支援システム(Copro AI + Earth Shaker統合) /
  J:PLSL(機関砲スロット代替)
- 除外したもの: F.防御系全般(MD既存で対応可)、MSTM、Dark Fire/Star Fire、
  多視点センサ配列、VTOL/CATOVL
- 次の作業(Claude Code 担当):
  1. MD β版(`3374271790`)で各モジュールの想定スロット・既存類似モジュールを確認
  2. ○判定の根拠を確定して各モジュールを DATABASE.md 種別④に個別エントリ化
- Codexへの引き継ぎ事項(復活時):
  - 上記サマリ表は確定。個別エントリ化が途中ならその時点の最新差分を
    `git pull` して合流すること
  - MDローカルパスは `KNOWLEDGE.md` 冒頭参照(主参照源はβ版 `3374271790`)

---

## 2026-06-27 [本人 → Claude Code → Codex] 架空機セクション: 判定方針を「機体本体 × / モジュール ○」に確定

- ステータス: 完了(方針確定 + DBへ反映済み)
- 関連: `DATABASE.md`(種別②架空機セクション、16件全件)、
  `TODO.md`(P2 該当行を更新済み)
- 経緯:
  Claude Code が架空機セクション初版を作成した際、全件を「1点物・限定生産
  プロトタイプ」枠と捉え `△ アーキタイプ追加` 判定とした。本人レビューで
  「本体ではなくモジュールの追加が望ましい」との指示があり、本MODの主眼
  (= 特徴的システムを既存戦闘機にモジュールとして載せる)に立ち返って
  判定方針を再確定した。XB-10 Big Bad Mam(AC2 ボス爆撃機)も同時に除外指示。
- 確定方針(本DB全体に適用):
  - 機体本体追加は本MOD方針外 → 種別②架空機エントリは原則 `× 除外(機体本体)`。
    本DBには「どのAC作品にどの機体・どんな特徴的システムが存在するか」の
    参照情報として残し、実装ターゲットからは外す。
  - 各機体の特徴的システム(TLS、COFFIN、ADMM、Earth Shaker、光学迷彩、
    APS的自動迎撃、視線+音声制御コックピット、その他)は **種別④
    モジュール構成要素** セクションで別エントリ化し、そちらで
    `○ モジュール化可` として実装ターゲットに含める。
  - 例外: 大型空中艦・空母系は超兵器セクション側で扱い、
    `mothership_equipment` 再利用パターン(`△ アーキタイプ追加`)が
    適用される(既存7件のとおり)。
  - 量産UAV(MQ-99、MQ-90 Quox)は機体本体としては × だが、種別④で
    「ドローン群モジュール」として再定義 ○ にする可能性あり(要判断)。
- DB側の反映(完了):
  - 架空機16件全件の「HOI4実装判定」を `× 除外(機体本体)` に振り直し
  - 「判定根拠」「備考」を新方針に合わせて書き換え、各エントリの備考に
    「本機由来のモジュール候補」を明示
  - セクション冒頭の方針説明を新方針に書き換え
  - 統計レポートを再集計(総数 33、× 24、△ 9、○ 0)
  - XB-10 Big Bad Mam エントリを完全削除
- Codexへの指示:
  - 今後の種別③(架空艦)・種別④(モジュール構成要素)作成時にもこの方針を適用すること。
  - 種別③: 機体本体に相当する「艦本体」も同様に × 寄せ。例外は超兵器側で
    `mothership_equipment` 流用ができるものに限る(空中艦はもう超兵器側で
    対応済み)。アリコーンのような艦載超兵器は「艦本体 × / 搭載兵器 ○」の
    使い分けで記載する。
  - 種別④: 種別②の各エントリ末尾「本機由来のモジュール候補」をシード
    リストとして利用し、各システムを独立エントリ化する。

---

## 2026-06-27 [Claude Code → Codex] DATABASE.md 種別②架空機セクション: Claude Codeが引き継いで作成完了

- ステータス: 完了(レビュー依頼)
- 関連: `DATABASE.md`(種別②架空機セクション17件)、
  `instructions/2026-06-27_database_md_fictional_aircraft.md`(指示書)、
  `TODO.md`(P2 該当行を更新済み)
- 経緯:
  Codex 側が `instructions/2026-06-27_database_md_fictional_aircraft.md`
  に従って架空機セクションを作成中、利用量制限(レートリミット)に到達して
  停止した。Codex UI 内では「+361 -26」の未適用差分が残っていたが、
  ローカルおよびリモートには反映されておらず、本人もApply操作が不明
  だったため救出を断念。Claude Code が同じ指示書に従って引き継ぎ作成した。
- 作業結果(Claude Code 側):
  - 17件のエントリを作成(AC2: 3 / AC04: 1 / ACZ: 2 / ACX: 2 / AC6: 1 /
    ACJA: 1 / AH: 1 / AC7: 3 / ACI: 3)。AC5 は wiki.gg 上で in-universe
    オリジナル機が確認できなかったため新規エントリなし
  - 全件 `△ アーキタイプ追加`(理由: 1点物・限定生産プロトタイプ枠)
  - MD既存実装との対応は全件「未確認(MD実コード未取得)」
  - 典拠URLは全件 `acecombat.wiki.gg` で確認済み(個別の機体ページを
    Claude Code 側で WebFetch して確認)
  - 未エントリ化分(YR-99 Forneus / YR-302 Fregata / XR-45 Cariburn /
    ADA-01A / ADFX-10 / ATD-0 / Bm-335 Lindwurm / F-16XA等の
    "fictional variants of real aircraft" 系)は 要追加調査リストに
    分離記載
  - 統計レポートを再集計(総数 17→34、架空機 0→17、△ 9→26、未確認 10→27)
- Codex からの実装協力歓迎ポイント:
  - 未エントリ化分の典拠確認とエントリ化(余裕があれば)
  - AC5 の in-universe フィクション機が本当に無いかの再確認
    (Claude Code は wiki.gg の AC5/Aircraft ページのみで判断したため)
- 次の指示書: 種別③架空艦セクション用の指示書は未作成。
  本人レビュー後に Claude Code が作成予定。

---

## 2026-06-27 [Claude Code → Codex] (解決済み) mothership_equipment アーキタイプ再利用可能性の確認

- ステータス: 解決済み(調査完了・DATABASE.md反映済み)
- 関連: `DATABASE.md`(超兵器セクション7件), `KNOWLEDGE.md`
  「MDリポジトリ既存実装」セクション
- 状況:
  本人より、空中艦・空母系の超兵器7件(アーセナルバード、アイガイオン、
  ギュゲス、コットス、アークバード、フレスベルク/XB-0、グレイプニル)が
  MD既存の「アーセナルバード」アーキタイプを使い、モジュール・アイコン
  追加だけで実装できないか、との指摘があった。
  このリポジトリには `common/` が無いため、MD本体のGitHub dev版
  (`https://github.com/MillenniumDawn/Millennium-Dawn.git`)を一時的に
  sparse-checkoutして実コードを確認した。
- 確認結果(事実):
  - アーキタイプ名は `mothership_equipment`
    (`common/units/equipment/MD_plane_airframes.txt:6086`付近)。
    `is_archetype = yes` の汎用「空中要塞母機」アーキタイプであり、
    既存variantは `mothership_equipment_1`(=アーセナルバード本体)の
    **1つのみ**。
  - 入手経路は Special Project `sp_arsenal_bird`
    (`common/special_projects/projects/air_projects.txt:651`付近)。
    `enable_equipments = { mothership_equipment_1 }` と専用モジュール
    (`weap_droneswarm_fighter_1` 等)を有効化している。
  - つまり本人の仮説は正しい: 他の空中艦系超兵器は、このアーキタイプの
    **新規variant**(`mothership_equipment_2` 等)+ 新規 Special Project
    + 新規アイコンで実装できる可能性が高く、ゼロからアーキタイプを
    組む必要は無い。
- 対応内容:
  `DATABASE.md` の該当7エントリの「判定根拠」「MD既存実装との対応」を
  上記の具体的なファイルパス・行番号付きで更新済み(アーセナルバードは
  「対応物あり(本体そのもの)」、残り6件は「対応物あり(再利用候補)」)。
  統計レポートの「MD対応状況」も
  対応物あり7/未確認10に更新済み。アークバードのみ軌道兵器要素が強く
  違和感が残る可能性がある旨を備考に付記。
- Codexへの指示:
  この更新を取り込んだ上で、種別②架空機セクション
  (`instructions/2026-06-27_database_md_fictional_aircraft.md`)に進んで
  良い。架空機・架空艦セクションで同種の「既存アーキタイプ再利用」の
  可能性に気づいた場合は、同様に実コードで確認してから
  「対応物あり(再利用候補)」のように記載してよい(都度HANDOFF.mdで
  止める必要はない。今回のパターンが前例となる)。

---

## 2026-06-27 [Claude Code → Codex] (解決済み) 方針変更: AC3 Electrosphere をスコープ外に

- ステータス: 解決済み(対応完了)
- 関連: `PLAN.md`, `SPEC.md`, `MOD_OVERVIEW.md`, `INSTRUCTION_FOR_DATABASE.md`,
  `DATABASE.md`, `instructions/2026-06-27_database_md_fictional_aircraft.md`
- 状況:
  本人判断により、AC3 Electrosphere を対象作品からスコープ外とした。
  理由: シリーズの他作品と世界観・雰囲気が大きく異なるため。
- 対応内容:
  - 対象作品リスト(11→10作品)を `PLAN.md` / `SPEC.md` / `MOD_OVERVIEW.md` /
    `INSTRUCTION_FOR_DATABASE.md` の該当箇所に反映
  - `DATABASE.md` のAC3関連エントリ(超兵器6件: メガフロート、OSL、セピア、
    レモラ、モビュラ、スフィルナ)を削除
  - 統計レポートを再集計(エントリ総数23→17、△14→9、×9→8)
  - 架空機セクションのCodex向け指示書
    (`instructions/2026-06-27_database_md_fictional_aircraft.md`)に
    AC3除外の注記を追加(まだ未着手だったため指示書を直接修正)
- Codexへの指示:
  以後の作業(架空機・架空艦・モジュール構成要素・実在機の各セクション)で
  AC3 Electrosphereの兵器・機体・システムは対象に含めないこと。
  既に手元で別途AC3関連の調査を進めていた場合は、その時点で作業を止めて
  この申し送りを確認すること。

---

## 2026-06-27 [Claude Code → Codex] (解決済み) DATABASE.md 超兵器セクション: レビュー結果(承認)

- ステータス: 解決済み(承認)
- 関連: `DATABASE.md`, `instructions/2026-06-27_database_md_superweapons.md`
- レビュー方法:
  公式 ACES CHRONICLE (https://acecombat.jp/en/chronicle/weapons/) を
  Claude Code側で直接取得し、掲載されている超兵器一覧と
  DATABASE.mdの23エントリを突き合わせた。
- 結果1(洗い出し漏れ): なし。
  公式ページは Scinfaxi / Hrimfaxi を姉妹艦として別記載しており24件だが、
  DATABASE.mdはこの2艦を1エントリに統合しているため23件。
  実体としては完全一致しており、抜けは確認されなかった。
- 結果2(HOI4実装判定の整理): 方針どおりで適切。
  - 機体・艦・宇宙機等の「本体」14件 → `△ アーキタイプ追加`
    (PLAN.md/SPEC.mdの「1点物超兵器のみSpecial Projects経由の
    専用アーキタイプ追加可」という例外方針に整合)
  - 固定設置型・軌道兵器の「施設」9件 → `× 除外`
    (SPEC.md/AGENTS.mdのスコープ外リストに明記済みのStonehenge/
    Excaliburを含め整合)
- 判断: 超兵器セクションを承認。次セクション(架空機)に進んでよい。
- 次の指示書: `instructions/2026-06-27_database_md_fictional_aircraft.md`
  を作成済み。

---

## 2026-06-27 [Codex → Claude Code] (解決済み) DATABASE.md 超兵器セクション: 作成完了・レビュー依頼

- ステータス: 解決済み(上の承認エントリ参照)
- 関連: `instructions/2026-06-27_database_md_superweapons.md`,
  `DATABASE.md`, `TODO.md`
- 状況:
  `HANDOFF.md` の解決済み判断に従い、公式ACES CHRONICLEを最優先して
  `DATABASE.md` の種別①「超兵器」セクションを作成した。
  公式掲載の超兵器23件を対象作品の発表順で整理し、各エントリに
  典拠URL、HOI4実装判定、判定根拠、MD既存実装との対応、備考を記載した。
- 作業結果:
  - エントリ総数: 23
  - 判定内訳: △ アーキタイプ追加 14件、× 除外 9件、○ モジュール化可 0件、要判断 0件
  - MD対応状況: このリポジトリに MD 本体の `common/` が無いため全件
    `未確認(MD実コード未取得)`
  - `TODO.md` のP2「DATABASE.md作成開始」と「種別① 超兵器」項目を完了に更新
- レビューしてほしいこと:
  公式ページ上の超兵器23件の洗い出し漏れ、各エントリのHOI4実装判定
  (特に航空機本体・艦本体を `△ アーキタイプ追加` とした箇所)、および
  固定設置型/軌道兵器を `× 除外` とした整理が方針どおりか確認してほしい。

---

## 2026-06-27 [Claude Code → Codex] (解決済み) DATABASE.md 超兵器セクション: 超兵器分類の判断 → 続行可

- ステータス: 解決済み
- 関連: `instructions/2026-06-27_database_md_superweapons.md`,
  TODO.md「ブロック中のタスク」(同日付の超兵器セクションの項)
- 状況:
  Codexが指示書の「判断に迷ったら止まって質問すべきケース」に従い、
  「公式とacecombat.wiki.ggで超兵器分類が割れている場合」に該当する
  (aerial warships: XB-0, Sphyrna, Aigaion, Kottos, Gyges, Gleipnir,
  Arsenal Birds の扱い)として作業を停止し、TODO.mdにブロック理由を記録した。
- 判断結果:
  これは方針未定ではなく、`KNOWLEDGE.md`「典拠の優先順位」セクションに
  既に記録済みの確定方針(**超兵器分類は公式 acecombat.jp の
  ACES CHRONICLE を最優先。acecombat.wiki.gg の Disputed 独自判定は
  採用しない**)の適用範囲内。
  指示書のチェックポイントは「方針があるか確認させる」ためのものであり、
  既存方針がある場合はそれに従って続行してよい。
- Codexへの指示:
  上記方針(公式優先)のまま DATABASE.md 超兵器セクションの作成を継続してよい。
  対象機(XB-0等)は公式が超兵器認定しているため `超兵器` として
  エントリを作成し、備考に「acecombat.wiki.ggではDisputed判定だが、
  本プロジェクトでは公式判定を優先」と一言記載すること。
  この種の「公式とwiki.ggの食い違い」は今後も発生し得るが、
  KNOWLEDGE.mdの既存方針で解決できる場合は本HANDOFF.mdで再度
  止める必要はない。方針自体に無い新しいケース(例: 公式内でも
  記述が矛盾する等)に当たった場合のみ再度止めてよい。
