# HANDOFF.md — エージェント間の申し送り

本ドキュメントは Claude Code と Codex 間の作業引き継ぎメモである。
`TODO.md`(タスクの状態管理)とは役割が異なり、ここには
「なぜ止まったか」「次に誰が何を判断・実行すべきか」を記録する。
会話ログそのものではない。運用ルールは `CLAUDE.md` / `AGENTS.md` の
「9. エージェント間の申し送り」を参照。

新しいエントリは先頭に追記する。解決済みエントリは削除せず、
見出しに `(解決済み)` を付けて残す。

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
