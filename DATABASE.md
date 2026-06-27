# DATABASE.md — AC兵器・実装候補の根拠台帳

本ドキュメントは ACM-MD で扱う兵器・システムの根拠台帳である。
ここに無い情報を MOD 実装の根拠として使わない。

作成ルールは `INSTRUCTION_FOR_DATABASE.md` に従う。

---

## 種別 ① 超兵器

典拠の優先順位:

- 超兵器分類は公式 ACES CHRONICLE を最優先する。
- acecombat.wiki.gg の Disputed 判定は、本プロジェクトでは公式判定に優先しない。

MD既存実装との対応について:

- このリポジトリには MD 本体の `common/` が無いため、原則 MD既存実装との
  対応は `未確認(MD実コード未取得)` とする。
- ただし2026-06-27、Claude Code が GitHub dev版
  (MillenniumDawn/Millennium-Dawn)を一時的にsparse-checkoutして
  `mothership_equipment` アーキタイプ周辺を確認した。空中艦・空母系の
  超兵器7件(アークバード、フレスベルク、グレイプニル、アイガイオン、
  ギュゲス、コットス、アーセナルバード)は、この既存アーキタイプの
  新規variant追加で実装できる可能性が高いことが分かったため、
  該当エントリのみ「対応物あり」に更新済み。詳細は HANDOFF.md /
  KNOWLEDGE.md 参照。

公式典拠:

- https://acecombat.jp/en/chronicle/weapons/

**注記(2026-06-27)**: AC3 Electrosphere は対象作品からスコープ外に
決定されたため、AC3関連の超兵器6件(メガフロート、OSL、セピア、レモラ、
モビュラ、スフィルナ)は削除済み。経緯は `PLAN.md` / `SPEC.md` 参照。

---

### ストーンヘンジ (Stonehenge)

- **種別**: 超兵器
- **登場作品**: AC04 / AC7
- **製造主体(in-universe)**: 中央ユージア連合 (Federation of Central Usea) 主導の国際共同事業
- **性質**: 小惑星ユリシーズの破片迎撃を目的とした大型レールガン網。
  AC04ではエルジア軍が戦略兵器として転用した。AC7では残存砲台が
  ライトハウス戦争で再利用された。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定設置型超兵器であり、本MODのスコープ外。
  艦船・航空機の既存スロットへ載せるモジュールとしては扱わない。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 固定設置型超兵器は判定×だが、判断材料としてDBには残す。

### メガリス (Megalith)

- **種別**: 超兵器
- **登場作品**: AC04
- **製造主体(in-universe)**: エルジア連邦共和国 (Federal Republic of Erusea)
- **性質**: 軌道上のユリシーズ破片を人工的に落下させるための巨大要塞。
  レーザー誘導システムとロケット、ミサイルサイロを備える。構造物としての
  防御力が高い。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定要塞であり、本MODの「既存装備へのモジュール追加」方針外。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 固定設置型超兵器は判定×だが、判断材料としてDBには残す。

### シンファクシ級潜水空母 (Scinfaxi & Hrimfaxi)

- **種別**: 超兵器
- **登場作品**: AC5
- **製造主体(in-universe)**: ユークトバニア連邦共和国 (Union of Yuktobanian Republics)
- **性質**: シンファクシ (Scinfaxi) とリムファクシ (Hrimfaxi) の大型原子力潜水空母。
  長時間潜航、VLSからのバーストミサイル発射、航空機運用が可能で、
  海上・空中・地上目標へ脅威を与える。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 艦本体に相当するため、モジュール単体では再現しにくい。
  搭載兵装やVLS能力は将来モジュール化候補になり得る。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 潜水空母本体はスコープ判断が必要になりやすい。兵装・航空運用能力を
  切り出す方が本MODの方針に近い。

### アークバード (Arkbird)

- **種別**: 超兵器
- **登場作品**: AC5
- **製造主体(in-universe)**: オーシア連邦 (Osean Federation) / ユークトバニア連邦共和国
  (Union of Yuktobanian Republics) の共同計画
- **性質**: 低軌道を周回する大型宇宙機。もともとはユリシーズ破片の処理を
  目的としたが、環太平洋戦争中に兵器化された。レーザーとミサイルを用いる。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 宇宙機本体に相当し、通常装備モジュールではない。
  既存の `mothership_equipment` アーキタイプ(下記参照)を再利用した
  新規variant追加で実装できる可能性がある。新規アーキタイプ構築は不要と推定。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: 軌道兵器要素が強く、衛星本体を航空機アーキタイプで表現する
  ことに違和感が残る可能性はあるが、メカニクス上は流用可能。
  実装する場合は本人と方向性を確認すること。

### SOLG (Strategic Orbital Linear Gun)

- **種別**: 超兵器
- **登場作品**: AC5
- **製造主体(in-universe)**: オーシア連邦 (Osean Federation)
- **性質**: 冷戦期の戦略防衛構想で開発された軌道上の大型レールガン。
  地上攻撃用の弾頭・高速ミサイルを発射できる。後にグレイメンが再利用した。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 衛星・軌道兵器であり、既存装備デザイナーのモジュールとして
  扱う方針外。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: レールガンという要素だけを艦船モジュールに流用する場合は、
  別途「モジュール構成要素」で扱う。

### エクスキャリバー (Excalibur)

- **種別**: 超兵器
- **登場作品**: ACZ
- **製造主体(in-universe)**: ベルカ公国 (Principality of Belka)
- **性質**: タウバーグ南部の丘陵地帯に建設された高高度レーザー兵器。
  中央塔、複数のレーザー塔、発電施設、妨害施設からなる固定システムで、
  弾道ミサイル防衛構想の一部として設計された。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定設置型超兵器であり、本MODのスコープ外。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 固定設置型超兵器は判定×だが、判断材料としてDBには残す。

### フレスベルク (Hresvelgr / XB-0)

- **種別**: 超兵器
- **登場作品**: ACZ
- **製造主体(in-universe)**: 南ベルカ国営兵器産業廠 (South Belka Munitions Factory)
- **性質**: ベルカ戦争中に秘密開発された重巡航管制機。A World With No Boundaries の
  移動作戦基地として使われた。巨大な翼幅を持ち、多数の戦闘機を搭載できる。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 空中空母本体に相当する。既存の `mothership_equipment`
  アーキタイプ(下記参照)を再利用した新規variant追加で実装できる
  可能性がある。新規アーキタイプ構築は不要と推定。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。

### ネベラジャマー (Nevera Jammer)

- **種別**: 超兵器
- **登場作品**: ACX
- **製造主体(in-universe)**: オーレリアの気象観測レーダーをレサス軍 (Leasath forces) が転用
- **性質**: ネベラ山脈にある広域妨害施設。元は気象観測レーダーだったが、
  レサス軍がジャミング施設へ転用した。広範囲の通信・レーダー妨害で
  重要拠点を防護する。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定施設であり、本MODのモジュール追加方針外。
  電子戦効果だけを将来モジュール化する場合は別扱い。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 電子戦モジュールの参考にはなるが、施設本体は実装対象外。

### メソンカノン (Meson Cannons)

- **種別**: 超兵器
- **登場作品**: ACX
- **製造主体(in-universe)**: オーレリア (Aurelia)
- **性質**: オーレリア首都のアトモスリングに組み込まれた荷電粒子ビーム兵器。
  首都防衛用で、弾道ミサイルや長距離攻撃への固定防衛に向く。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 都市防衛用の固定兵器であり、機体・艦船モジュールとして扱わない。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 粒子ビーム兵器の概念は将来別項目で再検討可能。

### アーケロン要塞 (Archelon Fortress)

- **種別**: 超兵器
- **登場作品**: ACX
- **製造主体(in-universe)**: レサス民主共和国 (Democratic Republic of Leasath)
- **性質**: セントリー島に秘密建設された軍事要塞兼生産施設。
  フェンリア (Fenrir) の生産施設であり、光学迷彩用のエネルギー供給施設と
  ショックカノンを備える。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定要塞・生産施設であり、本MODのモジュール追加方針外。
  ショックカノン等の構成要素は別項目で扱う余地がある。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本体は除外。搭載兵装・エネルギー供給要素は将来の候補。

### グレイプニル (Gleipnir)

- **種別**: 超兵器
- **登場作品**: ACX
- **製造主体(in-universe)**: レサス民主共和国 (Democratic Republic of Leasath)
- **性質**: 光学迷彩、衝撃波弾道ミサイル、ショックカノンを備える大型戦略飛行船。
  次世代機フェンリアの基礎にもなった。航空要塞として大きな脅威を与えた。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 航空要塞本体に相当するため、実装する場合はvariant追加が必要。
  既存の `mothership_equipment` アーキタイプ(下記参照)を再利用した
  新規variant追加で実装できる可能性がある。光学迷彩やショックカノンは
  モジュール化候補になり得る。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。

### シャンデリア (Chandelier)

- **種別**: 超兵器
- **登場作品**: AC6
- **製造主体(in-universe)**: エストバキア (Estovakia)
- **性質**: ユリシーズ破片の破壊を目的に開発された大型レールガン兵器。
  完成前に小惑星到達を迎え、後にエメリア・エストバキア戦争で兵器化された。
  要塞化された防御と強力な攻撃能力を持つ。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: × 除外
- **判定根拠**: 固定設置型超兵器であり、本MODのスコープ外。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: レールガン要素は将来の艦船モジュール候補になり得るが、
  シャンデリア本体は除外。

### アイガイオン (Aigaion)

- **種別**: 超兵器
- **登場作品**: AC6
- **製造主体(in-universe)**: エストバキア連邦共和国 (Federal Republic of Estovakia)
- **性質**: エストバキアの空中艦隊構想の中核となる重巡航管制機。
  ベルカ由来技術を基に設計され、ニンバス巡航ミサイル発射能力と空中空母能力を持つ。
  コットス、ギュゲスと連携して運用される。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 空中空母本体に相当する。既存の `mothership_equipment`
  アーキタイプ(下記参照)を再利用した新規variant追加で実装できる
  可能性がある。新規アーキタイプ構築は不要と推定。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。

### ギュゲス (Gyges)

- **種別**: 超兵器
- **登場作品**: AC6
- **製造主体(in-universe)**: エストバキア共和国 (Republic of Estovakia)
- **性質**: アイガイオンを中心とする空中艦隊構想の火力支援プラットフォーム。
  多連装砲とミサイルにより、広い選択肢の火力支援を行う。耐久性と機動性を備える。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 支援航空機本体に相当する。既存の `mothership_equipment`
  アーキタイプ(下記参照)を再利用した新規variant追加で実装できる
  可能性がある。新規アーキタイプ構築は不要と推定。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。

### コットス (Kottos)

- **種別**: 超兵器
- **登場作品**: AC6
- **製造主体(in-universe)**: エストバキア共和国 (Republic of Estovakia)
- **性質**: アイガイオンを中心とする空中艦隊構想の電子支援プラットフォーム。
  広域電子戦能力により、敵通信・レーダー妨害や味方航空機の防御支援を行う。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 支援航空機本体に相当する。既存の `mothership_equipment`
  アーキタイプ(下記参照)を再利用した新規variant追加で実装できる
  可能性がある。電子戦能力のみを切り出せばモジュール化候補にもなり得る。
- **MD既存実装との対応**: 対応物あり(再利用候補) —
  `mothership_equipment` アーキタイプ
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`、
  GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)に倣い、
  新規variant(`mothership_equipment_2`等)+専用Special Project+
  専用アイコンで実装可能と推定。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。

### アリコーン (Alicorn)

- **種別**: 超兵器
- **登場作品**: AC7
- **製造主体(in-universe)**: エルジア王国海軍ラーン艦隊 (Erusean Royal Navy's Rán Fleet) が運用
- **性質**: 原子力潜水艦であり、航空母艦と長距離レールガン発射プラットフォームを
  兼ねる潜水航空巡洋艦。潜航状態から遠距離攻撃を行うことを想定した大型艦。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 艦本体に相当するため、既存スロットへのモジュール追加だけでは
  再現できない。搭載レールガンやSLUAV発射機は別途モジュール候補。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 艦本体は判断が必要になりやすい。搭載兵装を切り出す方が
  本MODの方針に合う。

### アーセナルバード (Arsenal Birds)

- **種別**: 超兵器
- **登場作品**: AC7
- **製造主体(in-universe)**: オーシア連邦 (Osean Federation)
- **性質**: 国際軌道エレベーター防衛用に開発された巨大無人航空機。
  軌道エレベーターからのマイクロ波送電で長時間飛行し、補給船による補給を受ける。
  Liberty と Justice の2機が展開された。
- **典拠URL**:
  - https://acecombat.jp/en/chronicle/weapons/
  - https://acecombat.wiki.gg/wiki/Superweapon
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 巨大航空機本体に相当する。MDには既存実装 `mothership_equipment`
  があり、その唯一のvariant `mothership_equipment_1` がアーセナルバード
  そのものとして実装済み。本MODでは新規アーキタイプ構築は不要(既存実装を
  そのまま参照・流用する想定)。
- **MD既存実装との対応**: 対応物あり(本体そのもの) —
  `mothership_equipment` アーキタイプおよび variant `mothership_equipment_1`
  (`common/units/equipment/MD_plane_airframes.txt:6086-6251`)。
  Special Project `sp_arsenal_bird`
  (`common/special_projects/projects/air_projects.txt:649`付近)で
  `enable_equipments = { mothership_equipment_1 }` および専用モジュール
  (`weap_droneswarm_fighter_1`, `weap_droneswarm_bomber_1`,
  `weap_missile_bay_1`, `weap_droneswarm_anti_ship_1`,
  `fully_autonomous_computer_system_arsenal`)を有効化する実装が既に存在
  (GitHub dev版 MillenniumDawn/Millennium-Dawn で2026-06-27 Claude Code確認)。
- **備考**: acecombat.wiki.ggではDisputed判定だが、本プロジェクトでは公式判定を優先。
  アーセナルバード自体はMDに実装済みのため、本MODでの追加作業は
  既存モジュール群の確認・必要に応じた調整のみで済む可能性が高い。

---

## 種別 ② 架空機

本セクションは AC作品に登場する in-universe の架空航空機を扱う。
全機網羅ではなく、各作品の **主人公機・ライバル機・ボス機・特徴的システムを
持つ機体** に絞っている(指示書: `instructions/done/2026-06-27_database_md_fictional_aircraft.md`)。
通常の量産戦闘機は本MOD方針外のため、対象は基本的に「1点物・限定生産プロトタイプ」が中心。

MD既存実装との対応について:

- 本リポジトリには MD 本体の `common/` が無いため、全件「未確認(MD実コード未取得)」
  とする(超兵器セクション7件のような個別調査は未実施)。
- 架空機本体の多くは「1点物 + 特徴的システム」型で、超兵器セクションの
  `mothership_equipment` パターン(専用アーキタイプ + Special Project)に
  倣う想定。`△ アーキタイプ追加` を基本判定とする。

---

### ADF-01 FALKEN

- **種別**: 架空機
- **登場作品**: AC2(初出ボス機)、AC5・ACI・他複数作品で再登場
- **製造主体(in-universe)**: North Osea Gründer Industries(オーシア連邦)
- **性質**: 二発エンジンの先進制空戦闘機。COFFIN (Connection For Flight Interface)
  コックピットでパイロットが伏臥姿勢で操縦し、機体周囲16基のカメラ・センサで
  360度視認する。機首に化学レーザのTLS (Tactical Laser System) を内蔵し、
  発射時のみノーズが開く。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADF-01_FALKEN
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 限定生産プロトタイプの「1点物超兵器」相当。専用アーキタイプ
  + Special Project化(`mothership_equipment` パターン)が妥当。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: COFFIN・TLSはそれぞれ「モジュール構成要素」セクションで別エントリ化候補。

---

### XFA-27

- **種別**: 架空機
- **登場作品**: AC2(初出、プレイヤー機)、ACX で復活、ACI で再登場
- **製造主体(in-universe)**: 国・製造元ともに公式に非公開(機密扱い)
- **性質**: 可変翼(swing-wing)+ 全可動カナード + エンジン上下に計4枚の
  尾翼を持つ、推力偏向ノズル装備の実験戦闘攻撃機。COFFIN技術を一部採用し、
  コックピット下部にカメラを配置。短時間連射の標準ミサイル(後年作品では MSTM
  = Multiple-Launch Standard Missile)が特徴。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XFA-27
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: ユーセア連合軍が「最後の希望」として急造した実験機で、量産には
  至っていない設定。1点物・プロトタイプ枠。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 製造元・国籍が公式機密のため、HOI4側の国家所属付けは要判断。

---

### XB-10 Big Bad Mam

- **種別**: 架空機
- **登場作品**: AC2(初出、ボス機)、AHL(Assault Horizon Legacy)で再登場
- **製造主体(in-universe)**: 製造元不明(ユーセア地域の20世紀末に開発、
  反乱軍が施設を奪取)
- **性質**: 翼幅142mの全翼型大型試作爆撃機。後方機銃と空対空ミサイルを持ち、
  破壊に通常ミサイル6発以上を要する高耐久度が特徴。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XB-10_Big_Bad_Mam
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 大型実験爆撃機の「1点物枠」。専用アーキタイプ追加で対応。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 全翼型超大型爆撃機としては、超兵器セクションのアイガイオン級と
  並ぶ「巨大空中艦」候補だが、運用形態は通常の戦略爆撃機に近い。

---

### X-02 Wyvern

- **種別**: 架空機
- **登場作品**: AC04(初出)、AC5・ACI・他複数作品で登場
- **製造主体(in-universe)**: Erusean Air and Space Administration (EASA)/エルジア
- **性質**: 第5世代戦闘機。前進翼を畳んで抵抗を減らす可変ジオメトリ翼が最大の
  特徴。腹部の内部兵装ベイ + 機首両側エアインテーク内の小型ベイ。
  3次元推力偏向ERG-1000エンジン2基。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/X-02_Wyvern
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 開発が80年代開始 → 中断 → 1998再開のプロトタイプ枠。
  大陸戦争終結までに量産化されず、完成機は4〜6機程度。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 可変翼+ステルス+内部ベイの組み合わせ。後継機X-02S Strike Wyvernは
  別エントリ。

---

### ADFX-01 Morgan

- **種別**: 架空機
- **登場作品**: ACZ(初出、プレイヤー機)、ACI で再登場
- **製造主体(in-universe)**: South Belka Munitions Factory(ベルカ連邦)、1985開発
- **性質**: 前進翼+カナードの4.5世代実験機。TLS「Zoisite」(エンジン間支柱に
  化学レーザ搭載、機体保護のため7G制限)、Morganite ECM(敵ミサイル誘導阻害
  + 味方ミサイル強化)、MPBM (Multi-Purpose Burst Missile) の3点が特徴。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADFX-01_Morgan
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 単一機のみ製造、戦後オーシア連邦に接収された1点物プロトタイプ。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: TLS・MPBM・Morganite ECMはそれぞれ「モジュール構成要素」候補。
  後継ADF-01 FALKENはADFX-02の残骸から開発(設定上の系譜)。

---

### ADFX-02 Morgan

- **種別**: 架空機
- **登場作品**: ACZ(初出、ライバル機 / Pixy搭乗)
- **製造主体(in-universe)**: South Belka Munitions Factory(後の Gründer Industries)
- **性質**: ADFX-01と同設計だが、機体補強+全特殊兵装の同時搭載が可能。
  TLSは指向性制御(油圧 + 照準系)、ECMは正面以外の全方位からの弾道を逸らす。
  特殊兵装は Zoisite(TLS)+ Hypersthene(MPBM)+ Morganite(ECMポッド)の3点。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADFX-02_Morgan
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 1機のみ製造、最終ミッションで撃墜。残骸はGründer Industriesが
  回収しADF-01 FALKENの基礎となった、純粋な1点物。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: ADFX-01とアーキタイプ共用 + variant差分で実装する案あり
  (mothership_equipment と同パターン)。

---

### XFA-33 Fenrir

- **種別**: 架空機
- **登場作品**: ACX(初出、最終ボス)
- **製造主体(in-universe)**: Leasath Air Force New Weapon Laboratory(リーサス連邦、
  Diego Gaspar Navarro 体制下)
- **性質**: グレイプニル由来の光学迷彩(改良型、地上のエネルギー発信機を要する)、
  メインエンジン2基+中央可動エンジン1基によるVTOL、COFFIN制御、迷彩によるエンジン
  ノイズ低減。HPM(High-Powered Microwave、敵機エンジン過熱)とLSWM
  (Long Range Shock Wave Missile、グレイプニル弾道弾の縮小版)を搭載。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XFA-33_Fenrir
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: オリジナル機はアウレリア戦争で破壊。戦後アウレリア軍が残骸から
  簡易型(VTOL/光学迷彩なし)を再現した程度の1点物。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 光学迷彩・HPMは「モジュール構成要素」候補。VTOL機は HOI4 の
  通常航空機モデルでは表現困難な可能性あり(要判断)。

---

### XFA-24A Apalis

- **種別**: 架空機
- **登場作品**: ACX(初出)、以降ACJA・ACIで再登場(携帯機3作品のみ)
- **製造主体(in-universe)**: 不明
- **性質**: カナード + 推力偏向の試作汎用機。"Earth Shaker" ソフトウェア
  アップグレードによる地形デジタル解析で対地攻撃力を強化。コスト・部品の
  汎用性を重視した設計思想。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XFA-24A_Apalis
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: プロトタイプ多用途戦闘機。製造規模不明だが量産機ではない。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 製造元不明のためHOI4側の国家所属付けは要判断。

---

### CFA-44 Nosferatu

- **種別**: 架空機
- **登場作品**: AC6(初出、ボス機 / Ilya Pasternak 搭乗)、AH(DLC)、ACI再登場
- **製造主体(in-universe)**: Albastru-Electrice 国営企業(エストバキア連邦)
  ※AHでは旧ソ連がルーマニア工場で生産との別設定。ACI は不明
- **性質**: 内部兵装ベイ3個を持つ第5世代実験機。
  ADMM(All Direction Multi-purpose Missile、コード名 "Inferno"、最大12機を同時
  ロック)、EML(電磁射出機=レールガン、"Purgatorio")、ECM("Cocytus"、
  通常妨害機+センサ焼き切りビーム)、UAV-45s("Malebolge"、自律ドローン)を
  搭載可能。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/CFA-44_Nosferatu
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 開発開始2005年、エメリア=エストバキア戦争中に実験運用された
  少数機のみ。戦後もエストバキアが少数生産を継続するが量産機ではない。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: ADMM・EML・UAV制御の各システムはそれぞれ「モジュール構成要素」候補。
  AH/ACIでの製造主体の食い違いは備考に保持。

---

### GAF-1 Varcolac

- **種別**: 架空機
- **登場作品**: ACJA(初出、Varcolac Squadron 機)、ACI再登場
- **製造主体(in-universe)**: 不明(「Golden Axe Plan」名義で秘密開発、
  民間軍事会社・政府請負業者の関与示唆)
- **性質**: カナード + 後退翼一体型 + 双垂直尾翼の大型カナード機。
  サイズは通常の戦闘機の2倍以上。各パイロット個別のチューニング(例: Milosz
  Sulejmani 機は高機動性 + 自動迎撃ガンシステムによるミサイル迎撃)。
  QAAM・4AAM・LSWM(Long Range Shock Wave Missile)互換。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/GAF-1_Varcolac
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 秘密計画下で機体ごとに個別チューニングされた少数機。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 自動迎撃ガンシステム(対ミサイル防御)はAPS的システムとして
  「モジュール構成要素」候補。

---

### ASF-X Shinden II

- **種別**: 架空機
- **登場作品**: AH(初出、DLC)、ACI・AC7 で Strangereal 世界にも登場(2020-10-28)
- **製造主体(in-universe)**: Taiga Heavy Industries(2003開発開始、TFJ-01 GoldStar
  ビジネスジェットをベースとする)。実世界デザインは河森正治氏(マクロスシリーズ)
- **性質**: 前進翼 + カナードの可変ジオメトリ機。垂直積層の2基エンジンによる
  CATOVL(Catapult-Assisted Take-Off, Vertical Landing)能力。大型兵装ベイ4個と
  外部ハードポイント6箇所。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ASF-X_Shinden_II
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: AH 世界では2018年の Haneda Incident 後に本格運用入り、F-3 派生で
  日本・英国に配備という設定で、純粋な1点物ではないが、HOI4 1936-開始MD的には
  プロトタイプ枠として扱う方が自然。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: CATOVL は MD の艦載機 / VTOL 表現に依存。AH と Strangereal で世界線が
  違うため、in-universe 設定の使い分けは要判断。

---

### X-02S Strike Wyvern

- **種別**: 架空機
- **登場作品**: AC7(初出、Mihaly A. Shilage 搭乗)
- **製造主体(in-universe)**: Erusean Air and Space Administration + North Osea
  Gründer Industries 共同開発
- **性質**: X-02 Wyvern の現代化派生。可変ジオメトリ前進翼、ステルス強化、
  複座、コンフォーマル燃料タンク、3D推力偏向、耐熱マグネシウム合金エンジン、
  フライバイワイヤ + パワーバイワイヤ。中央兵装ベイに実験的 Arclight 電磁射出機
  (レールガン)搭載可、改良型 Dark Fire 中距離AAMと次世代 Star Fire 対艦ミサイル。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/X-02S_Strike_Wyvern
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 2010年開発開始、2019年灯台戦争時点で量産前。3Dメタルプリンタ製造
  のプロトタイプ枠。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: Arclight レールガンは「モジュール構成要素」候補(アリコーンの
  レールキャノンとは別物として扱う)。

---

### ADF-11F Raven

- **種別**: 架空機
- **登場作品**: AC7(初出、最終ミッション無人機 Hugin / Munin)、DLC で有人版
- **製造主体(in-universe)**: North Osea Gründer Industries + Erusean Air and Space
  Administration 共同開発
- **性質**: 第7世代モジュラー設計。ADF-11 ノーズユニット + RAW-F 翼ユニットの組合せ。
  ノーズは有人コックピットまたは分離可能なUAVを選択可能。Copro 搭載AI に
  Mihaly A. Shilage の戦闘データをロードし、高G旋回でパイロット負担を低減。
  特殊兵装は機首パルスレーザ砲、TLS、QAAM、翼下から最大2基の支援UAVを射出可能。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADF-11F_Raven
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 有人/無人切替式の1点物に近いプロトタイプ。無人版2機は最終
  ミッションで全機撃墜。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: ノーズユニット分離 + 子UAV射出は HOI4 の通常航空機モデルでは
  直接表現困難な可能性あり(要判断)。AI Copro システムは「モジュール構成要素」候補。

---

### MQ-99

- **種別**: 架空機
- **登場作品**: AC7(灯台戦争、エルジア側主力UAV)
- **製造主体(in-universe)**: Erusean Air and Space Administration(ベルカ製UAV
  技術 + North Osea Gründer Industries 技術ベース)
- **性質**: コンテナ射出式の戦闘UAV。機関砲 + 内部兵装ベイ(AAM/対地兵器選択)。
  9G旋回可能だが脆弱で、ミサイル1発か機関砲一連射で撃墜される。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/MQ-99
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 無人機。MDの既存UAV/ドローン枠の有無未確認(要grep)。
  群運用前提のため、量産可UAVとして実装する案もあり(その場合は ○ に降格可)。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: プレイヤー使用不可。本MODでも「敵専用 / 量産可能」枠で扱うか判断要。

---

### QFA-44 Carmilla

- **種別**: 架空機
- **登場作品**: ACI(初出、Butterfly Master 制御)
- **製造主体(in-universe)**: 不明
- **性質**: CFA-44 Nosferatu の無人化派生。Butterfly Master の衛星経由で
  COFFIN リモート制御。CFA-44 同様の内部兵装ベイに ADMM。MQ-90L Quox bis UAVと
  ネットワークし、CIWS的にレーザ防御も可能。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/QFA-44_Carmilla
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: CFA-44派生の無人プロトタイプ。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: CFA-44 とアーキタイプ共用 + variant差分で実装する案あり。
  衛星リモートCOFFINと UAV ネットワーク制御は「モジュール構成要素」候補。

---

### MQ-90 Quox

- **種別**: 架空機
- **登場作品**: ACI(原作初出は小説『Ace Combat: Ikaros in the Sky』内 "Q-X")
- **製造主体(in-universe)**: 日本航空自衛隊 / 設計: 山本平次郎
- **性質**: テイルレスW翼型UAV。可変翼ブレード装備。GPS航法、亜音速で
  高機動だが超音速巡航不可。GAU-19機関砲(機首左)、内部ベイにAAM-4
  (空対空)または対地兵装。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/MQ-90_Quox
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: 無人UAV。ACI設定では2010年代に米国等へ輸出されており、
  量産機扱い。MQ-99と並ぶUAVカテゴリで、量産可UAVとして実装する案もあり
  (○に降格可)。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 制式採用UAVのため、本MODで量産解放するか「特定国専用」とするかは
  要判断。

---

### ADA-01B ADLER

- **種別**: 架空機
- **登場作品**: ACI(初出、UNF/傭兵側)
- **製造主体(in-universe)**: 製造国不明。設計図を Wernher and Noah Enterprises
  が取得し、UNF傘下傭兵が複製生産
- **性質**: ADF-01系の派生。COFFIN(視線移動 + 音声コマンド制御)、ガラスキャノピー
  ではなく360度ディスプレイ内蔵カメラ式コックピット。前縁翼+大型インテーク。
  ADA-01A の SDBM翼ランチャーを廃し、機首にMPBM (Multi-Purpose Burst Missile)
  発射口を搭載して機動性を向上。副兵装は MGP(機関砲ポッド)・4AGM。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADA-01B_ADLER
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠**: ACI 限定機。Strangereal 本編未登場で、ACI 設定限定の少数生産機。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 視線+音声制御 COFFIN は「モジュール構成要素」候補。

---

## 要追加調査リスト

### MD実コード未取得のため未確認

このリポジトリには MD 本体の `common/` 配下が無いため、以下のエントリで
MD既存実装との対応は未確認(2026-06-27に個別確認した空中艦・空母系7件を除く)。

- ストーンヘンジ (Stonehenge)
- メガリス (Megalith)
- シンファクシ級潜水空母 (Scinfaxi & Hrimfaxi)
- SOLG (Strategic Orbital Linear Gun)
- エクスキャリバー (Excalibur)
- ネベラジャマー (Nevera Jammer)
- メソンカノン (Meson Cannons)
- アーケロン要塞 (Archelon Fortress)
- シャンデリア (Chandelier)
- アリコーン (Alicorn)

### 確認済み(2026-06-27、Claude CodeがMD GitHub dev版を一時取得して確認)

以下7件は `mothership_equipment` アーキタイプ
(`common/units/equipment/MD_plane_airframes.txt:6086-6251`)の新規variant
追加で実装できる可能性が高いことを確認済み。「対応物あり」として記録。

- アークバード (Arkbird)
- フレスベルク (Hresvelgr / XB-0)
- グレイプニル (Gleipnir)
- アイガイオン (Aigaion)
- ギュゲス (Gyges)
- コットス (Kottos)
- アーセナルバード (Arsenal Birds) — variant `mothership_equipment_1` として実装済み

### 架空機セクション 未エントリ化分(本DBで個別エントリ未作成)

2026-06-27 時点で、acecombat.wiki.gg に独立ページが存在することは確認済みだが、
詳細(製造主体・特徴的システム)の典拠確認を後回しにした架空機。
今回の選定基準(主人公機・ライバル機・ボス機・特徴的システム機を各作品3-8機)
ではエントリ化必須ではないと判断したが、後続のモジュール構成要素セクション
作成時に「特徴的システムを持つ」と判明した時点で本DBへ追加する。

- YR-99 Forneus(ACX)
- YR-302 Fregata(ACX/ACJA で再登場)
- XR-45 Cariburn(ACX)— アニメ『戦闘妖精雪風』Mave に酷似のデザイン参照あり
- ADA-01A ADLER(ACI)— ADA-01B との差分のみエントリ化候補
- ADFX-10 / ADFX-10F Prototype Raven(ACI)
- ATD-0(典拠不確定)
- Bm-335 Lindwurm(典拠不確定)
- F-16XA Sakerfalcon / F-16XF Gyrfalcon / F-22C Raptor II 等の "fictional variants
  of real aircraft" 系(ACI 中心)— 種別「実在機」セクションで扱う方が自然と判断

---

## 統計レポート

(2026-06-27更新: 種別②架空機セクションを追加(17件)。
全件「1点物・限定生産プロトタイプ」枠で `△ アーキタイプ追加`、
MD対応は MD実コード未取得のため全件「未確認」)

- エントリ総数: 34
- 種別別件数:
  - 超兵器: 17
  - 架空機: 17
  - 架空艦: 0
  - モジュール構成要素: 0
  - 実在機: 0
- 判定別件数:
  - ○ モジュール化可: 0
  - △ アーキタイプ追加: 26
  - × 除外: 8
  - 要判断: 0
- MD対応状況:
  - 対応物あり: 7
  - 該当なし: 0
  - 未確認: 27
- 要追加調査リストの件数: 18(超兵器10件 + 架空機未エントリ化8件、確認済み7件を除く)
