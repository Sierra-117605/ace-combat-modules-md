# 指示書: P5 試作モジュール第2弾 — TLS (Tactical Laser System)

- 発行者: Claude Code
- 発行日: 2026-06-28
- 担当: Codex
- ステータス: 未対応
- 優先度: **高**(P5 第2弾、本MODの「特徴的システム」感を強める主役モジュール)

---

## 目的

本MOD ACM-MD の P5 試作第2弾として、TLS(Tactical Laser System、機載化学
レーザ砲)を **2 tier 構造** で実装する。これは本MOD最初の **複数tier
モジュール** であり、`parent` / `can_convert_from` の正しい使い方を実地検証
する意味もある。

最終的に HOI4 装備デザイナーで:
- 通常戦闘機(`small_plane_airframe` 系)の **主兵装スロット** または
  **補助兵装スロット** に「TLS - Zoisite」「TLS - Directional」が選択肢として現れる
- 上位tier(`acm_tls_2`)が「`acm_tls_1` からの改修」として選べる
- 性能修正値(air_attack, air_agility 等)が反映される
- アイコンは PLSL と同じ `special_counter_4` 由来(レーザ系統一)

---

## 前提・作業前にやること

1. **git pull** で最新を取り込む(直近: `8c7d602`、本人指示で
   plane_fighter_weapons カテゴリへ統一済み)
2. 以下を読む(順序重要):
   - `DATABASE.md` 種別④の **TLS** エントリ(本実装の根拠台帳。
     2026-06-28 改訂後の `plane_fighter_weapons` カテゴリ指定が反映済み)
   - `SPEC.md` 3.1〜3.9(モジュール定義書式、ローカライズキー、アイコン、SP)
     と 4.3(スロット可否表、TLS が `plane_fighter_weapons` の
     `fixed_main_weapon_slot/auxiliary` で受入)
   - `KNOWLEDGE.md`「MDモジュール定義書式」「外部MODアイコンの流用テクニック」
     「MOD配置とランチャー認識の落とし穴」「マブラブMOD移植プロジェクトからの
     転用知見」13項目
   - `acm-md/common/units/equipment/modules/acm_plane_modules.txt`
     (既存の PLSL 実装。同じ書式に揃えて TLS 2件を **追記**)
   - `acm-md/interface/acm_plane_modules.gfx`(既存の PLSL 用 sprite 登録に
     TLS 2件分を追記)
   - `acm-md/localisation/english/acm_plane_modules_l_english.yml` と
     `acm-md/localisation/japanese/acm_plane_modules_l_japanese.yml`
     (既存に追記)
3. Steam β版MD `3374271790` の `MD_plane_modules.txt` で
   `plane_fighter_weapons` カテゴリの既存モジュール
   (例: `weap_aam_1` 〜、`MD_plane_modules.txt:2125〜`)を **数件サンプリング**
   して、`add_stats` / `build_cost_resources` / `xp_cost` の数値水準を把握する
   (TLS のバランス調整はこれを参考に上方寄せ)

---

## 実装するモジュール仕様

### TLS - Zoisite(基礎tier)

| 項目 | 値 |
|------|-----|
| ID | `acm_tls_1`(prefix `acm_` 必須、ハイフン不可) |
| abbreviation | `tls1` |
| category | `plane_fighter_weapons` |
| sfx | `sfx_ui_sd_modules_turret` |
| add_equipment_type | `fighter` |

`add_stats`(初期目安、Codex で MD既存 fighter_weapons 数値を見て微調整):
- `air_attack = 15`(レーザの瞬間火力、AAM2発分相当)
- `air_agility = -3`(重量増 + 7G airframe 制限)
- `air_defence = -2`(脆弱な兵器装備のため被弾耐性低下)
- `build_cost_ic = 60`(実験兵器級)
- `reliability = -0.08`(化学レーザの安定性低)
- `supply_consumption = 0.02`

`build_cost_resources`:
- `chromium = 2`(レーザ装置)
- `microchips = 2`(照準系)
- `composites = 1`(軽量化)

`xp_cost = 25`

### TLS - Directional(上位tier)

| 項目 | 値 |
|------|-----|
| ID | `acm_tls_2` |
| abbreviation | `tls2` |
| category | `plane_fighter_weapons` |
| sfx | `sfx_ui_sd_modules_turret` |
| parent | `acm_tls_1` |
| add_equipment_type | `fighter` |

`add_stats`:
- `air_attack = 25`(指向制御で命中率↑、ADFX-02 強化)
- `air_agility = -2`(制限緩和)
- `air_defence = -1`(機体補強で被弾耐性回復)
- `build_cost_ic = 100`
- `reliability = -0.05`(改良で信頼性↑)
- `supply_consumption = 0.02`

`build_cost_resources`:
- `chromium = 3`
- `microchips = 3`
- `composites = 2`

`can_convert_from`:
- `module = acm_tls_1`
- `convert_cost_ic = 50`

`xp_cost = 25`

---

## ファイル構成と編集箇所

**新規ファイル作成は不要**。既存ファイルに追記するだけ。

### 1. `acm-md/common/units/equipment/modules/acm_plane_modules.txt`

既存の `acm_pulse_laser_1 = { ... }` ブロックの **下に** `acm_tls_1` と
`acm_tls_2` を追記する。同じ `equipment_modules = { limit = { has_dlc = "By
Blood Alone" } ... }` ブロック内に並べる。

### 2. `acm-md/interface/acm_plane_modules.gfx`

既存の `GFX_EMI_acm_pulse_laser_1` の **下に** 2件追記:

```
spriteType = {
    name = "GFX_EMI_acm_tls_1"
    texturefile = "gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds"
}
spriteType = {
    name = "GFX_EMI_acm_tls_2"
    texturefile = "gfx/interface/technologies/0_modules/plane_builder/support/special_counter_4.dds"
}
```

PLSL と同じ `special_counter_4.dds` を再参照(レーザ系統一、本人指示)。
追加する .dds 実体は **無し**(texturefile 直接参照、SPEC.md 3.8 方針)。

### 3. ローカライズ

**`acm-md/localisation/english/acm_plane_modules_l_english.yml`** に追記:

```
 acm_tls_1: "Tactical Laser System (TLS) - Zoisite"
 acm_tls_1_desc: "Chemical laser system mounted between engines. Codenamed 'Zoisite' on the ADFX-01 Morgan prototype. Imposes a 7G airframe limit to protect the vulnerable weapon system, but delivers high instantaneous firepower. Trades off with a standard missile load."
 acm_tls_2: "Tactical Laser System (TLS) - Directional"
 acm_tls_2_desc: "Improved TLS with hydraulic targeting for directional control. Originally fitted on ADFX-02 Morgan and inherited by the ADF-01 FALKEN. Reinforced airframe relaxes the G-limit."
```

**`acm-md/localisation/japanese/acm_plane_modules_l_japanese.yml`** に追記:

```
 acm_tls_1: "戦術レーザシステム (TLS) - ゾイサイト"
 acm_tls_1_desc: "エンジン間に搭載される化学レーザ。ADFX-01 モーガン試作機での「ゾイサイト」コード名。脆弱な兵器を保護するため機体に7G制限がかかるが、瞬間火力は極めて高い。通常のミサイル兵装とトレードオフ。"
 acm_tls_2: "戦術レーザシステム (TLS) - 指向制御型"
 acm_tls_2_desc: "油圧+照準系で発射方向を制御できるTLS改良版。ADFX-02 モーガンに搭載され、後の ADF-01 FALKEN にも継承された。機体補強でG制限が緩和されている。"
```

ファイル先頭の `l_english:` / `l_japanese:` 行はそのまま。各行の先頭に
**半角スペース1個**(KNOWLEDGE.md ローカライズキー命名規則の慣例どおり)。
保存時に **UTF-8 BOM 付き** にすること(マブラブMOD移植知見 項目10)。

---

## 動作確認手順(本人実施、Codex の作業範囲外)

Codex は acm-md/ 配下のファイル追記 + commit/push まで。本人は以下を実施
(MOD コピーは本人が前回 OneDrive\ドキュメント\... に配置済みのため、上書き
コピーで OK):

1. `acm-md/` 配下を HOI4 MOD フォルダ
   (`C:\Users\tkmuh\OneDrive\ドキュメント\Paradox Interactive\Hearts of Iron IV\mod\acm-md\`)
   に上書きコピー
2. HOI4 ランチャーで MD β版 + ACM-MD を有効化(既に有効化済みなら再起動だけで OK)
3. ゲーム起動 → 任意の国 → 装備デザイナー → 戦闘機(small_plane / medium_plane 系)
4. **主兵装スロット**(`fixed_main_weapon_slot`)または **補助兵装スロット**
   (`fixed_auxiliary_weapon_slot_1` / `_2`)を開く
5. 選択肢の中に:
   - **「Tactical Laser System (TLS) - Zoisite」**
   - **「Tactical Laser System (TLS) - Directional」**
   - 日本語ロケールなら「戦術レーザシステム (TLS) - ゾイサイト」「指向制御型」
   が現れることを確認
6. アイコンが PLSL と同じ電子対抗手段最上位 tier(`special_counter_4`)になっているか目視
7. 「Directional」が「Zoisite」からの **改修選択肢**(`convert from acm_tls_1`)として現れるか確認
8. 装備設計を保存して autosave 通過すること
9. `logs/error.log` で `acm` / `tls` 関連のエラー無し

---

## 完了条件(Codex の作業範囲)

- `acm-md/common/units/equipment/modules/acm_plane_modules.txt` に
  `acm_tls_1` / `acm_tls_2` の2モジュール定義が追記されている
- `acm-md/interface/acm_plane_modules.gfx` に対応する2 sprite 登録が追記されている
- ローカライズ英日2ファイルに2件ずつの表示名・説明文が追記されている
  (BOM 付き UTF-8 維持)
- `acm_tls_2` が `parent = acm_tls_1` + `can_convert_from = { module =
  acm_tls_1, convert_cost_ic = 50 }` で正しく上位tierとして関連付けられている

---

## 完了後の手順

1. commit メッセージに本指示書ファイル名を含める
   例: `[Codex] P5試作 TLS実装 (ref: instructions/2026-06-28_p5_implementation_tls.md)`
2. 本ファイルを `instructions/done/` に移動して commit
3. `TODO.md` の P5 該当箇所に TLS 完了行を追加(PLSL のすぐ下)
4. `HANDOFF.md` 冒頭に
   `[Codex → Claude Code / 本人] P5 TLS 実装完了、本人動作確認待ち` として報告
5. `git push` してから手放す

---

## 判断に迷ったら止まって質問すべきケース

- MD既存の `plane_fighter_weapons` カテゴリの数値水準と比較して、本指示書の
  add_stats が「強すぎ」または「弱すぎ」と判断した場合、HANDOFF.md で
  推奨数値を提示して本人判断を仰ぐ
- `parent` / `can_convert_from` の書式で予想外の挙動を発見した場合
  (KNOWLEDGE.md「MDモジュール定義書式」セクションの「上位tierでの
  `parent` と `can_convert_from`」に既存実装例の行番号がある)
- ローカライズキー命名規則で `_desc` ではなく独自 suffix が必要そうな場合
  (KNOWLEDGE.md「ローカライズキー命名規則」参照、現状は `<ID>_desc` で OK)
