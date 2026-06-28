# TODO.md — タスク管理

本ドキュメントは ACM-MD プロジェクトのタスクを管理する。
コンテキストリセット(セッション切れ)しても再開できるよう、
タスクは具体的に、依存関係を明示する。

## 凡例

- `[ ]` 未着手
- `[~]` 進行中
- `[x]` 完了
- `[!]` ブロック中(理由を明記)
- 各タスクには担当環境を [claude.ai] [Claude Code/Codex] [本人] で明示

---

## P0: 初期セットアップ

- [x] [claude.ai] PLAN.md / SPEC.md / TODO.md / KNOWLEDGE.md /
      MOD_OVERVIEW.md / INSTRUCTION_FOR_DATABASE.md /
      CLAUDE.md / AGENTS.md / README.md / .gitignore の初版作成
- [ ] [本人] GitHub上に `ace-combat-modules-md` リポジトリを作成
  - URL: https://github.com/Sierra-117605/ace-combat-modules-md
- [ ] [本人] claude.ai から出力されたZIPをダウンロード、PCで展開
- [ ] [本人] `git init` → 初回 commit → リモート追加 → push
  - 手順は同梱の README.md を参照
- [ ] [本人] Claude Code または Codex を起動し、リポジトリディレクトリで
      作業開始

---

## P1: 既存ナレッジの棚卸し

- [x] [本人 + Claude Code/Codex] 過去のマブラブ戦術機追加MOD移植プロジェクトの
      リポジトリ・ファイル一式を確認
  - リポジトリ: `C:\dev\MD_TSF_Submod`(GitHub: Sierra-117605/MD_TSF_Submod)
  - 当時の作業ログ・KNOWLEDGE がローカルに残っていることを確認
  - 転用可能な知見を本プロジェクトの KNOWLEDGE.md に転記済み(2026-06-27)
- [x] [Claude Code/Codex] マブラブMODの実装でハマったポイントを整理
  - TSF/マブラブ固有の内容(戦術機スロット構成、OS種別等)は対象外とし、
    HOI4/MDモジュール実装一般の技術的地雷13項目を抽出・転記済み
  - 詳細: KNOWLEDGE.md「マブラブMOD移植プロジェクトからの転用知見」セクション

---

## P2: DATABASE.md 作成

- [x] [Claude Code] INSTRUCTION_FOR_DATABASE.md を読み込み、
      Codex向け指示書を作成
      → `instructions/2026-06-27_database_md_superweapons.md`
- [x] [Codex] 上記指示書に従い、DATABASE.md の作成を開始
  - 超兵器分類の判断待ちで一時停止 → HANDOFF.md で解決済み、続行可
- [x] [Codex] 種別 ① 超兵器 のセクションを完成 → 進捗報告
- [x] [Claude Code] 超兵器セクションをレビュー
  - 公式ACES CHRONICLEページを直接取得し、洗い出し漏れなしを確認(24件中、
    Scinfaxi/Hrimfaxiの姉妹艦統合により23エントリ。実体は完全一致)
  - HOI4実装判定(△14件/×9件)もPLAN/SPEC.mdの方針(機体・艦本体は
    アーキタイプ追加例外扱い、固定設置型は除外)と整合
  - 承認。詳細は HANDOFF.md 参照
- [ ] [本人] 超兵器セクションの内容を確認・訂正(任意。Claude Codeレビュー済み)
- [x] [本人 → Claude Code] AC3 Electrosphere をスコープ外に決定(2026-06-27)
  - 理由: シリーズの他作品と世界観・雰囲気が大きく異なるため
  - 対応: PLAN.md / SPEC.md / MOD_OVERVIEW.md / INSTRUCTION_FOR_DATABASE.md
    の対象作品リストを11→10作品に更新。DATABASE.mdのAC3関連エントリ
    (超兵器6件: メガフロート、OSL、セピア、レモラ、モビュラ、スフィルナ)を
    削除し、統計レポートを再計算(23件→17件)
  - 架空機セクションのCodex向け指示書にも除外を反映済み
- [x] [Claude Code] 種別 ② 架空機 のCodex向け指示書を作成
      → `instructions/2026-06-27_database_md_fictional_aircraft.md`
      (AC3除外を反映済み)
- [x] [本人 → Claude Code] mothership_equipment アーキタイプの再利用可能性を調査
  - 本人の仮説(空中艦系超兵器7件をアーセナルバードのアーキタイプ
    流用で実装できないか)をMD GitHub dev版の実コードで確認し、正しいと判明
  - DATABASE.mdの該当7エントリ・統計レポートを更新。詳細はHANDOFF.md参照
- [x] [Claude Code] 種別 ② 架空機 のセクションを完成(16件、全件 × 除外(機体本体))
  - 経緯: 2026-06-27 Codex 側が利用量制限で停止したため、Claude Code が
    引き継いで作成。当初は全件 △ アーキタイプ追加としたが、本人指示
    「本体ではなくモジュールが望ましい」を受けて全件 × に振り直した。
    XB-10 Big Bad Mam は本人指示で除外。詳細は HANDOFF.md 参照
  - 未エントリ化分(YR-99 Forneus 等8件)は DATABASE.md 末尾の
    「要追加調査リスト」に分離記載
- [x] [本人 → Claude Code] 架空機の判定方針を確定(2026-06-27)
  - 機体本体追加は本MOD方針外(全件 × 除外)
  - 特徴的システムは種別④モジュール構成要素で ○ モジュール化可として実装
- [ ] [本人] 架空機セクションの内容を確認・訂正
- [x] [Claude Code] 種別 ③ 架空艦 のセクションを完成(2026-06-28 方針更新版)
  - 本人指示により、シンファクシ級・アリコーンを **新規「超大型潜水艦」
    アーキタイプの 2 variant** として扱う方針に確定
  - 種別①の該当エントリの判定根拠も更新済み(両艦とも mothership_equipment
    パターンの艦版翻案でアーキタイプ追加)
  - 在来型架空艦(Hubert級空母、Leasath 潜水艦 Proteus/Triton、Aegir Fleet)
    は除外維持
  - 艦載モジュール候補は MD既存と非競合な3件のみ採用(種別④に追加済み):
    SRC-03a超大型レールキャノン / 対空バースト弾頭ミサイル / SLUAV発射ベイ
  - 競合候補(200mm副砲、48基VLS、艦上滑走路、弾道ミサイルポート、二重ステルス)
    はMD既存モジュールで代用
- [ ] [本人] 架空艦セクションの内容を確認・訂正
- [x] [Claude Code] 種別④ 艦載モジュール3件をエントリ化完了
- [x] [Claude Code + 本人] 種別 ④ モジュール構成要素 候補サマリを確定(14モジュール)
  - 2026-06-28 本人と協議の上、A〜J を整理して14候補に圧縮
  - F.防御系・MSTM・Dark Fire/Star Fire は除外
  - DATABASE.md 種別④セクション冒頭にサマリ表を追加済み
- [x] [Claude Code] 種別 ④ 各モジュールの個別エントリ作成完了(14件 全件 ○ モジュール化可)
  - MD β版パス: `C:\Program Files (x86)\Steam\steamapps\workshop\content\394360\3374271790\`
  - MD_plane_modules.txt で全カテゴリを確認し、14モジュールすべて
    既存カテゴリ(plane_avionics / plane_heavy_special_design_arsenal /
    plane_countermeasures / plane_fighter_weapons / plane_droneswarm_weapon /
    plane_drone_systems / plane_multipurpose_gun)に追加可能と判定
  - 種別④の個別エントリ + MD既存カテゴリマッピング表 をDATABASE.mdに記載済み
- [x] [Claude Code] 種別 ④ モジュール構成要素 のセクションを完成
  - 統計: 総数 33→47、○ 0→14、対応物あり 7→21
- [ ] [本人] モジュール構成要素セクションの内容を確認・訂正
- [ ] [Claude Code/Codex] 種別 ⑤ 実在機 のセクションを完成 → 進捗報告
- [ ] [本人] 実在機セクションの内容を確認・訂正
- [ ] [Claude Code/Codex] DATABASE.md 末尾に統計レポートを追記して完成

---

## P3: SPEC精査(MDコード調査)

- [x] [Codex] DBレビュー + モジュール定義書式精査 を完了
  - 指示書: `instructions/2026-06-28_db_review_and_p3_modules_spec.md`
  - Phase A: DATABASE.md 独立レビュー(自分で修正せずレビュー報告のみ)
  - Phase B: モジュール定義書式・stat一覧・modifier書式 を MD実コードから抽出
- [x] [Claude Code/Codex] MDのモジュール定義書式を精査
  - モジュールカテゴリの定義方法
  - 既存スロットに新カテゴリを許可する方法
  - 性能修正値(modifier)の書式
  - 利用可能な statキー一覧
- [x] [Codex] P3-A ローカライズキーの命名規則を確認
  - 指示書: `instructions/2026-06-28_p3_localization_keys.md`
- [x] [Codex] P3-B アイコン .dds 仕様 + .gfx 登録方法を確認
  - 指示書: `instructions/2026-06-28_p3_icon_dds_gfx.md`
  - サイズ・圧縮形式・登録先 `.gfx` ファイルパスを統合
- [x] [Codex] P3-C Special Projects の新規追加手順を確認
  - 指示書: `instructions/2026-06-28_p3_special_projects.md`
  - 完了で P3 SPEC精査フェーズ全体が完了
- [x] [Claude Code/Codex] 上記をすべて KNOWLEDGE.md に蓄積
- [x] [Claude Code/Codex] SPEC.md の「要調査」項目を「確定」に更新

---

## P4: 本命作品の選定

- [ ] [本人] DATABASE.md と SPEC.md を踏まえ、深掘りする本命AC作品を
      対象11作品から1つ選定
- [ ] [本人] 第一段階で実装するモジュール候補を1〜2個に絞る

---

## P5: 最小動作版(v0.1.0)の実装

- [x] [Codex] 最初のモジュール1個を実装(ファイル作成) — **PLSL** 試作
  - 指示書: `instructions/2026-06-28_p5_prototype_plsl.md`
  - 本MOD初の動く実装。本命作品選定(P4)前でも実装試行可能
- [x] [Codex] アイコンを既存流用 or 配置(PLSL試作の一部、上記指示書内)
- [x] [Codex] ローカライゼーション(英語、日本語)(PLSL試作の一部、上記指示書内)
- [x] [本人] HOI4 起動・MD β版 + ACM-MD のロード成功(2026-06-28、本人報告「問題ない」)
  - 解決過程の落とし穴は KNOWLEDGE.md「MOD配置とランチャー認識の落とし穴」に恒久記録
- [ ] [本人] 装備デザイナーで PLSL が機関砲スロットに表示・選択可能か確認
  - アイコンが `special_counter_4` 由来(電子対抗手段最上位 tier)になっているか目視確認
- [ ] [本人] ゲーム内で PLSL の性能修正値(air_attack +5 / air_agility -2 等)が適用されるか確認
- [ ] [本人] 動作OKなら v0.1.0 タグを切る

---

## P6: 拡張フェーズ(v0.1.0以降)

- v0.1.0 完成後に別途 PLAN.md / SPEC.md / TODO.md を更新する
- ここには現時点では書かない

---

## 並行タスク(随時)

- [ ] [Claude Code/Codex] ハマりポイント・地雷を見つけたら即 KNOWLEDGE.md に追記
- [ ] [claude.ai] 仕様変更・方針変更が出たら PLAN.md / SPEC.md /
      MOD_OVERVIEW.md を更新
- [ ] [本人] 進捗を Discord / メモ等に記録(任意)

---

## ブロック中のタスク

(現時点でブロック中のタスクなし。2026-06-27の超兵器分類判断待ちは
HANDOFF.mdで解決済み → P2セクションを参照)
