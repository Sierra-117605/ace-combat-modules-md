# 指示書: P5 試作モジュール 第1弾 — PLSL(Pulse Laser)

- 発行者: Claude Code
- 発行日: 2026-06-28
- 担当: Codex
- ステータス: 未対応
- 優先度: **高**(P3 成果の実地検証 + 本MOD初の動く実装)

---

## 目的

本MOD ACM-MD の **最初の動く実装** として、PLSL(Pulse Laser、機関砲スロット
代替モジュール)を1個実装し、本人が HOI4 を起動してゲーム内で動作確認できる
状態にする。

本指示書の達成は以下を意味する:

1. P3-A(ローカライズキー命名規則)、P3-B(アイコン .dds + .gfx 登録)、
   P3 モジュール定義書式 の **実地検証** が完了する
2. 「ACM-MD は MD のサブMODとしてロード可能で、モジュール1個が
   ゲーム内に出てくる」という最小の足場が確立する
3. 以降の P4 本命作品選定後の本格実装が、確立した足場の上で進められる

---

## 前提・作業前にやること

1. **git pull** で最新を取り込む(直近 commit: `85cff17`)
2. 以下を必ず読む(順序重要):
   - `DATABASE.md` の種別④「PLSL(Pulse Laser / 機関砲スロット代替)」
     エントリ(本実装の根拠台帳)
   - `SPEC.md` 3.1〜3.9 全部(モジュール定義書式、ローカライズキー命名規則、
     アイコン .dds + .gfx 登録、Special Projects 追加手順)
   - `KNOWLEDGE.md`「MDモジュール定義書式」セクションと、その下の P3-A〜C
     で追加されたサブセクション(命名規則・アイコン・SP)
   - `KNOWLEDGE.md`「マブラブMOD移植プロジェクトからの転用知見」13項目
     (特に項目2「.gfx 1ファイル sprite 数 約300未満」、項目10「.yml は BOM 必須」、
     項目6「ID にハイフン使用不可」、項目7「default_modules の slot category 不一致で
     autosave クラッシュ」は本実装で踏み外しやすい)
3. Steam β版MD (`...\workshop\content\394360\3374271790\`) の以下を参考実装として
   開いておく:
   - `common/units/equipment/modules/MD_plane_modules.txt`(`plane_multipurpose_gun`
     カテゴリの既存モジュール、行 1978〜)
   - `interface/plane_modules_icons.gfx`(モジュールアイコン .gfx 登録の例)
   - `localisation/english/plane_designer_l_english.yml` と
     `localisation/japanese/plane_designer_l_japanese.yml`

---

## 実装するモジュール仕様

### PLSL(Pulse Laser、機関砲スロット代替)

- **typescript ID**: `acm_pulse_laser_1`(prefix `acm_` 必須、ハイフン不可)
- **abbreviation**: `plsl`(4文字目安、生文字列)
- **category**: `plane_multipurpose_gun`(MD既存カテゴリ、`MD_plane_modules.txt:1978`〜)
- **意味**: 機関砲スロットを置き換える短時間バースト発射型レーザ。
  射程5000m、雲で減衰。AC7 で ADF-11F Raven / DarkStar が標準装備、
  F-15C / MiG-31B / Su-57 に special pod として搭載可能(典拠:
  https://acecombat.wiki.gg/wiki/Pulse_Laser)。
- **add_stats(初期目安、Codex で微調整可)**:
  - `air_attack = +5`(機関砲としてやや高め)
  - `air_agility = -2`(レーザ砲のため重量増)
  - `build_cost_ic = +25`
  - `reliability = -0.05`(実験兵器のため)
- **build_cost_resources**:
  - `chromium = 1`, `microchips = 1`
- **xp_cost**: 15
- **sfx**: 既存の機関砲 sfx(例: `sfx_ui_sd_module_turret`)を流用
- **天候減衰の再現**: `add_stats` で `night_penalty = +0.05`(雲・夜間で命中率低下を
  簡易表現)。完璧な「雲減衰」再現は HOI4 メカ上難しいため簡易再現でよい

### スロット適合の確認(重要)

本モジュールが既存のMD航空機アーキタイプの **どの機関砲スロットに乗るか** を
確認すること。Steam β版MD の標準的な戦闘機アーキタイプ(例: `small_plane_airframe`、
`medium_plane_airframe`、`small_plane_cas_airframe` 等)の `module_slots` 内で
`plane_multipurpose_gun` を受け入れるスロット(例: `fixed_main_weapon_slot`)が
あるか確認し、もし無ければ本MODから当該アーキタイプの `allowed_module_categories`
を **拡張する** 形で実装すること。

ただし、**MD本体の descriptor.mod が `common/units/equipment` を `replace_path`
宣言している**(`SPEC.md` 1.2 参照)ため、アーキタイプ本体を上書きすると本MODが
MD全体のアーキタイプを丸ごと置き換える形になり危険。対策:

- 既存スロットが既に `plane_multipurpose_gun` を受け入れる場合は何もしなくてよい
  (本MODはモジュール追加だけで済む)
- 既存スロットが受け入れない場合のみ、アーキタイプ上書きが必要。その場合は
  HANDOFF.md で本人判断を仰ぐ(本指示書では止める)

---

## ファイル構成(本MODディレクトリ構造)

本MOD自体の MOD ディレクトリは、本人のローカルMOD配置場所が決まっていないため、
**リポジトリ直下に `acm-md/` サブディレクトリ** を作って、その下に MOD ファイルを
配置する。本人はこれをローカルの HOI4 MOD フォルダ(通常
`C:\Users\<user>\Documents\Paradox Interactive\Hearts of Iron IV\mod\acm-md`)に
コピー or シンボリックリンクすればロードできる。

```
acm-md/
  descriptor.mod                   # MOD認識用
  common/
    units/
      equipment/
        modules/
          acm_plane_modules.txt    # PLSL モジュール定義
  interface/
    acm_plane_modules.gfx          # アイコン sprite 登録
  gfx/
    interface/
      equipments/
        modules/
          acm_plane_modules/
            GFX_EMI_acm_pulse_laser_1.dds  # アイコン本体(48x48 RGBA32 推奨)
  localisation/
    english/
      acm_plane_modules_l_english.yml      # 英語ロケール
    japanese/
      acm_plane_modules_l_japanese.yml     # 日本語ロケール
```

リポジトリ直下のサブディレクトリ案にした理由: 開発リポジトリと MOD 実体を
1つの git 管理下に置けて、commit 履歴で実装変更を追える。本人がローカルにコピー
するときも `acm-md/` ごとコピーすれば済む。

### descriptor.mod の最小例

```
version="0.0.1"
name="ACM-MD (Ace Combat Modules for Millennium Dawn)"
supported_version="1.19.*"
tags={
    "Gameplay"
    "Balance"
}
dependencies={
    "Millennium Dawn: A Beta Test Mod"
}
```

(Steam β版の `descriptor.mod` の `name="Millennium Dawn: A Beta Test Mod"` に
合わせる必要がある。Steam 正式版に切り替える場合は `name` 文字列を変更すること)

### アイコンの扱い

- **第1案(推奨、軽量)**: Steam β版MD の既存機関砲アイコンを **コピーして** 本MOD
  配下に置き、ファイル名だけ `GFX_EMI_acm_pulse_laser_1.dds` に変更。中身は流用。
  目視で「PLSL らしい見た目」になっていなくても動作確認には十分
- **第2案**: 48x48 RGBA32 単色プレースホルダー .dds を生成(例: 青色塗りつぶし)。
  ImageMagick 等で生成可能だが、Codex の実行環境で .dds が作れない場合は第1案に倒す
- **アイコン .gfx 登録**: P3-B の調査結果通り、`GFX_EMI_<module_id>` 形式で
  `interface/acm_plane_modules.gfx` に登録する

### ローカライズ

- P3-A 調査結果通り、`ID` が表示名、`ID_desc` が説明文
- 英語ファイル `acm_plane_modules_l_english.yml`:
  ```
  l_english:
   acm_pulse_laser_1: "Pulse Laser (PLSL)"
   acm_pulse_laser_1_desc: "High-spec anti-air/anti-surface laser firing short bursts. Replaces the standard machine gun. Range approx. 5,000m, attenuated by cloud cover. Equipped on ADF-11F Raven and DarkStar in AC7."
  ```
- 日本語ファイル `acm_plane_modules_l_japanese.yml`:
  ```
  l_japanese:
   acm_pulse_laser_1: "パルスレーザー (PLSL)"
   acm_pulse_laser_1_desc: "短時間バースト発射型の対空・対地用高出力レーザ。機関砲スロットを置き換える。射程約5000m、雲で減衰。AC7 で ADF-11F Raven / DarkStar に標準装備。"
  ```
- **両ファイル UTF-8 BOM 付きで保存**(KNOWLEDGE.md 項目10)
- ファイル名は `acm_` prefix で MD既存と衝突しないこと(項目9)

---

## 検証の手順(本人向け、Codex の作業範囲外)

Codex の作業範囲はファイル一式を作って commit/push までで完了。本人は以下の手順で
動作確認する(これは別途実施):

1. `acm-md/` 配下を HOI4 MOD フォルダにコピー
2. HOI4 ランチャーで MD β版 + ACM-MD を両方有効化
3. ゲーム起動 → 任意の国 → 装備デザイナー → 戦闘機 → 機関砲スロット
4. PLSL が選択肢に出るか確認
5. PLSL を選んで保存 → 設計が無効化されないか、autosave クラッシュしないか
6. `logs/error.log` と `logs/game.log` を起動後に確認(エラーがあれば本人が
   Claude Code / Codex に報告)

---

## 完了条件(Codex の作業範囲)

- `acm-md/` 配下に上記6種のファイルが作成されている
- ローカライズ .yml は UTF-8 **BOM 付き**で保存されている
- アイコン .dds は実在し(流用 or 生成)、`.gfx` 登録が一致している
- `descriptor.mod` で本MODが MD β版に依存していると宣言されている
- スロット適合に問題があった場合は HANDOFF.md で停止して判断を仰いだ状態
  (アーキタイプ上書き必要が判明したケース)

---

## 完了後の手順

1. commit メッセージに本指示書ファイル名を含める
   例: `[Codex] P5試作 PLSL実装 (ref: instructions/2026-06-28_p5_prototype_plsl.md)`
2. 本ファイルを `instructions/done/` に移動して commit
3. `TODO.md` の P5 該当チェックボックス
   「[ ] [Claude Code/Codex] 最初のモジュール1個を実装(ファイル作成)」を `[x]`、
   アイコン項目とローカライズ項目も `[x]` に更新
4. `HANDOFF.md` 冒頭に `[Codex → Claude Code / 本人] P5 試作 PLSL 実装完了、
   本人による動作確認待ち` として報告
   - 確認手順(上記検証の手順)を再掲する
   - スロット適合の確認結果(MD既存戦闘機アーキタイプのどのスロットで使えるかの
     具体名)を必ず記載
5. `git push` してから手放す

---

## 判断に迷ったら止まって質問すべきケース

- スロット適合確認で、本MOD単独(モジュール追加のみ)では PLSL が乗らない
  ことが判明した場合(MD アーキタイプ上書きが必要 → 本人判断要)
- .dds 作成手段が無く、既存流用も困難な場合
- MD β版と Steam 正式版で `plane_multipurpose_gun` の扱いが異なる場合の
  本MOD対応方針(両対応か β版優先か)
- 本実装中に MD実装で予想外の挙動を発見した場合(KNOWLEDGE.md 追記候補)
