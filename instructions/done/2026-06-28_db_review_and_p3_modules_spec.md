# 指示書: DATABASE.mdレビュー + P3 モジュール定義書式精査

- 発行者: Claude Code
- 発行日: 2026-06-28
- 担当: Codex
- ステータス: 未対応

---

## 目的

(1) Claude Code がここ2日間で大幅に動かした DATABASE.md(種別①②③④)を、
Codex 視点で **独立レビュー** し、整合性・典拠妥当性・ハルシネーション疑い箇所を
洗い出す。

(2) その上で、P3「SPEC精査」の最優先項目である
**MDモジュール定義書式・利用可能stat・modifier書式** を実コードから抽出して
`SPEC.md` / `KNOWLEDGE.md` に系統的に蓄積する。これは P5 v0.1.0 実装の前提知識
として最重要。

---

## 前提・作業前にやること

1. **git pull** で最新を取り込む(直近 commit: `f6de904` 補助レールガン追加)
2. 以下のドキュメントを順に読む(Claude Code が更新した最新情報):
   - `HANDOFF.md`(冒頭の 2026-06-28 エントリ4件 — 本人方針確定・判定振り直し
     経緯・MD実装ギャップ・超大型潜水艦アーキタイプ案・MQ-99削除等)
   - `TODO.md`(P2 全完了・P3 未着手の状態)
   - `KNOWLEDGE.md`(冒頭「MDローカル本体のパス」セクション。
     主参照源は Steam β版 `3374271790`)
   - `DATABASE.md`(全体)

---

## Phase A: DATABASE.md 独立レビュー

### A-1. 整合性チェック

以下の整合性を機械的に確認:

- **エントリ総数と統計レポートの数字が一致するか**
  (現在: 総数51 / 超兵器17 / 架空機16 / 架空艦0 / モジュール構成要素18(航空機13+艦載5) /
  実在機0)
- **種別①超兵器の判定別件数** (× 8 / △ 9 / ○ 0) が実際の各エントリ判定と合うか
- **種別②架空機の判定** が全件 `× 除外(機体本体)` になっているか
  (MQ-99 と MQ-90 Quox は「× / 要判断」と併記)
- **種別④モジュール構成要素の判定** が全件 `○ モジュール化可` になっているか
- **MD既存実装との対応** に書かれているファイル名・行番号が、Steam β版
  (`C:\Program Files (x86)\Steam\steamapps\workshop\content\394360\3374271790\`)で
  実在し、内容が記述と一致するか(サンプリング検査でよい、全件不要)

### A-2. 典拠の妥当性チェック

- 各エントリの `典拠URL` が wiki.gg / acecombat.jp / fandom のいずれかであり、
  リンク先が実在するか(サンプリング検査、5-10件程度で良い)
- 判定根拠や備考に **「~らしい」「~と思われる」等の推測表現** が混入していないか
  全文grepで確認:
  ```
  grep -nE "らしい|思われる|可能性が高い|だろう|であろう" DATABASE.md
  ```
- 「未確認」「不明」と書くべきところに具体情報が捏造されていないか
  (特に Claude Code は機体性能・搭載兵装の細部で誤情報を入れがちなので注意)

### A-3. 方針整合性チェック

本人確定方針との整合:

- **「機体本体追加は本MOD方針外」** が種別②全件 と 種別③振り分け表 で守られているか
- **「アリコーン・シンファクシ級は超大型潜水艦アーキタイプの2 variant」** という
  種別①の判定根拠が、種別③の設計案セクションと整合しているか
- **「F.防御系・MSTM・Dark Fire/Star Fire は除外」**「**MQ-99群運用UAVは除外**」が
  種別④全体で守られているか
- **「アイガイオン級空中艦・グレイプニル・アーセナルバード・アークバードは
  mothership_equipment 流用」** が種別①該当エントリと整合しているか

### A-4. 報告(Phase A 結果)

レビュー結果を `HANDOFF.md` 冒頭に新規エントリ
`[Codex → Claude Code] DATABASE.md 独立レビュー結果` で追記。
発見した不整合・要修正点をリストアップ。
**自分で修正してはいけない**(レビュー段階)。修正の要否は次セッションで
Claude Code または本人が判断する。

---

## Phase B: P3 モジュール定義書式の系統的調査

Phase A の報告後、本人または Claude Code から GO サインが出てから着手すること。
GO がない場合は Phase A 完了時点で作業終了し、`HANDOFF.md` に「Phase B 着手判断
待ち」と記載して停止。

### B-1. 調査対象

Steam β版 `3374271790` の以下ファイルを系統的に読む:

- `common/units/equipment/MD_plane_airframes.txt`(航空機アーキタイプ)
- `common/units/equipment/modules/MD_plane_modules.txt`(航空機モジュール)
- `common/units/equipment/MD_mtg_ships.txt`(艦アーキタイプ、特に
  `mothership_equipment` / `attack_submarine` / `missile_submarine` /
  `carrier` を重点)
- `common/units/equipment/modules/MD_ship_modules.txt`(艦モジュール)

### B-2. 抽出する情報

`KNOWLEDGE.md`「MDリポジトリ既存実装」セクションに以下を系統的に追記:

1. **モジュール定義の必須フィールド一覧**
   (`abbreviation`, `category`, `sfx`, `xp_cost`, `add_equipment_type`,
   `allow_mission_type`, `add_stats`, `build_cost_resources`, `multiply_stats`
   等の意味と必須/任意)
2. **`add_stats` で使える stat キー一覧**
   (航空機: `air_superiority`, `air_attack`, `air_agility`, `air_defence`,
   `air_range`, `naval_strike_attack`, `naval_strike_targetting`, ... 等)
   (艦: `naval_speed`, `surface_detection`, `sub_attack`, ... 等)
   既存モジュール定義から実際に使われている stat を全件洗い出す
3. **`build_cost_resources` の有効リソース名**
   (aluminium / steel / chromium / microchips / composites / rubber 等)
4. **`module_count_limit` の書式と既存使用例**
5. **`upgrades` ブロックの仕組み**(既存 upgrade 例: `plane_bba_radar_upgrade`,
   `plane_bba_engine_upgrade` 等)

### B-3. 報告(Phase B 結果)

調査結果を `KNOWLEDGE.md` に追記した上で、`HANDOFF.md` に
`[Codex → Claude Code] P3 モジュール定義書式調査結果` として追記。
`TODO.md` の P3 該当チェックボックス(モジュール定義書式精査の項)を更新。

`SPEC.md` の「要調査」項目のうち、本調査でカバーされた部分を「確定」に更新。
カバーしきれなかった項目(アイコン .dds、ローカライズ規則、Special Projects
新規追加手順)は P3 残課題として TODO.md に明示。

---

## 完了後の手順

1. commitメッセージに本指示書ファイル名を含める
   例: `[Codex] DATABASE.mdレビュー (ref: instructions/2026-06-28_db_review_and_p3_modules_spec.md)`
2. 本ファイルを `instructions/done/` に移動して commit
3. `HANDOFF.md` を更新
4. `git push` してから手放す

---

## 判断に迷ったら止まって質問すべきケース

- Phase A レビュー中に、Claude Code が書いた既存エントリの判定そのものに
  異論がある場合(例: 「これは○ではなく△ではないか」「典拠が弱い」等)
  → 自分で書き換えず、`HANDOFF.md` で異論として提示し本人判断を仰ぐ
- Phase B 調査中に MD実装が DATABASE.md と矛盾する事実を発見した場合
  → 同じく `HANDOFF.md` で本人判断を仰ぐ
- 本指示書から判断できない仕様変更や方針の必要が出た場合
  → 自己判断で進めず HANDOFF.md に申し送り
