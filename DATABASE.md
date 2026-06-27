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
  - https://acecombat.wiki.gg/wiki/Scinfaxi-class_submarine
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠(2026-06-28 更新)**: 本MODで新規追加する **「超大型潜水艦」
  アーキタイプ** の variant として実装。同アーキタイプはアリコーンと共通化し、
  シンファクシ級・アリコーン間の差別化はモジュール選択で行う。
  詳細は **種別③ 架空艦** セクション参照。
- **MD既存実装との対応**: アーキタイプ自体は新規追加が必要(MD既存に該当なし、
  既存の `attack_submarine` / `missile_submarine` は通常規模で代替不可)。
  ただしアーキタイプの構造は航空機の `mothership_equipment` パターンを
  艦版に流用すれば構築可能。
- **備考**: シンファクシ級独自のモジュール候補は **SOFFM 対空バースト弾頭
  ミサイル**(burst warhead特性、種別④に追加済み)。
  ステルス材料(対ソナー+対レーダ二重)は MD既存ステルス枠と競合のため除外。

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
  - https://acecombat.wiki.gg/wiki/Alicorn
- **HOI4実装判定**: △ アーキタイプ追加
- **判定根拠(2026-06-28 更新)**: 本MODで新規追加する **「超大型潜水艦」
  アーキタイプ** の variant として実装(シンファクシ級と共通化)。
  シンファクシ級・アリコーン間の差別化はモジュール選択で行う。
  詳細は **種別③ 架空艦** セクション参照。
- **MD既存実装との対応**: アーキタイプ自体は新規追加が必要(MD既存に該当なし)。
  ただしアーキタイプの構造は航空機の `mothership_equipment` パターンを
  艦版に流用すれば構築可能。
- **備考**: アリコーン独自のモジュール候補は **SRC-03a 超大型レールキャノン**
  (600mm、核砲弾発射可)と **SLUAV 発射ベイ**(潜水艦から空中無人機を射出)。
  両者は MD既存と非競合のため種別④に追加済み。
  200mm 副砲レールガン / 48基VLS / 艦上滑走路は MD既存
  (`module_railguns_category` / `module_vls_category` /
  `module_flight_decks_category`)と直接競合のため独自モジュール化はせず、
  MDの既存モジュールを既存tier最大で割り当てる想定。

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

HOI4実装判定の方針(2026-06-27 本人確定):

- 本MODの主眼は「**特徴的システム**(TLS, COFFIN, ADMM 等)を**既存戦闘機に
  モジュールとして追加する**」こと。**機体本体の追加は方針外**。
- したがって本セクションのエントリは原則 `× 除外(機体本体)` とする。
  本DBには「どのAC作品にどの機体・どんな特徴的システムが存在するか」の
  参照情報として残し、実装ターゲットからは外す。
- 各機体の特徴的システムは、後続の **種別④モジュール構成要素** セクションで
  別エントリ化し、そちらで `○ モジュール化可` として実装ターゲットに含める。
- 例外: 大型空中艦・空母系は超兵器セクション側で扱い、
  `mothership_equipment` 再利用パターン(`△ アーキタイプ追加`)が適用される。

MD既存実装との対応について:

- 本リポジトリには MD 本体の `common/` が無いため、全件「未確認(MD実コード未取得)」
  とする(超兵器セクション7件のような個別調査は未実施)。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(COFFIN、TLS、16基カメラ・センサ配列)は種別④モジュール
  構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: COFFINコックピット、機首化学レーザTLS、
  多視点センサ配列。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(可変翼+カナード、推力偏向、限定COFFIN、MSTM連射機能)は
  種別④モジュール構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 機体形状(可変翼+4枚尾翼)はモジュールで再現困難。
  MSTM(短時間連射ミサイル系)も種別④モジュール候補からは除外
  (2026-06-28 本人判断: MD既存ミサイルtierで充足)。
  製造元・国籍が公式機密のため国家所属付けは要判断。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  本機の特徴は機体形状そのもの(可変ジオメトリ翼)に集約されるため、
  単独モジュール化候補は限定的。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: モジュール候補が限定的な代表例。3D推力偏向ERG-1000エンジン、
  内部兵装ベイ程度。後継機X-02S Strike Wyvernは別エントリ。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(TLS Zoisite、Morganite ECM、MPBM)は種別④モジュール
  構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補3点(TLS Zoisite、Morganite ECM、MPBM)は
  ベルカ系技術ツリーの基幹候補。後継ADF-01 FALKENはADFX-02の残骸から開発
  (設定上の系譜)。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  ADFX-01からの差分(指向性TLS、全方位ECM、全特殊兵装同時搭載)は
  種別④モジュール構成要素で別エントリ化対象(ADFX-01モジュール群の上位tier)。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: ADFX-01の上位tierとしてベルカ系技術ツリーの最終段階候補。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(光学迷彩、VTOL、HPM、LSWM)は種別④モジュール構成要素で
  別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: 光学迷彩(地上発信機要件付き)、
  HPMマイクロ波兵器、LSWM。VTOL機構は HOI4 の通常航空機モデルでは
  表現困難な可能性あり(要判断)。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(Earth Shaker、ODMM)は種別④モジュール構成要素で
  別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: Earth Shaker(地形デジタル解析による
  対地攻撃強化)、ODMM(全方位多目的ミサイル)。製造元不明。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム4種(ADMM、EML、ECM、UAV-45s)は種別④モジュール構成要素で
  別エントリ化対象。本MODのモジュール実装の最重要参照機。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補4点: ADMM "Inferno"(12機同時ロック)、
  EML "Purgatorio"(連射レールガン)、ECM "Cocytus"(センサ焼き切りビーム付)、
  UAV-45s "Malebolge"(自律ドローン)。
  AH/ACIでの製造主体の食い違いは設定整理時に再確認要。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(自動迎撃ガンシステム)は種別④モジュール構成要素で
  別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: 自動迎撃ガンシステム(対ミサイル防御=
  航空機版APS)。機体形状そのもの(超大型カナード機)はモジュール化困難。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  本機の特徴は機体形状(可変ジオメトリ前進翼+CATOVL)に集約され、
  単独モジュール化候補は限定的。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: モジュール候補が限定的な代表例。CATOVL機構はMDのVTOL/艦載機表現
  との整合確認が必要。AH と Strangereal で世界線が違うため、in-universe
  設定の使い分けは要判断。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(Arclight電磁射出機、Dark Fire/Star Fireミサイル)は
  種別④モジュール構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: Arclight機載レールガン
  (アリコーン艦載レールキャノンとは別物)。改良型 Dark Fire / Star Fire
  ミサイルは種別④モジュール候補から除外(2026-06-28 本人判断: 通常ミサイル
  tier の改良として既存枠に吸収)。可変翼自体はモジュール化困難。

---

### ADF-11F Raven

- **種別**: 架空機
- **登場作品**: AC7(初出、最終ミッション無人機 Hugin / Munin)、DLC で有人版
- **製造主体(in-universe)**: North Osea Gründer Industries + Erusean Air and Space
  Administration 共同開発
- **性質**: 第7世代モジュラー設計。ADF-11 ノーズユニット + RAW-F 翼ユニットの組合せ。
  ノーズは有人コックピットまたは分離可能なUAVを選択可能。Copro 搭載AI に
  Mihaly A. Shilage の戦闘データをロードし、高G旋回でパイロット負担を低減。
  機関砲スロットにはPLSL(Pulse Laser、機銃の代替)を標準装備。
  特殊兵装はTLS、QAAM、翼下から最大2基の支援UAVを射出可能。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADF-11F_Raven
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(Copro AI、機首パルスレーザ、TLS、支援UAV射出)は
  種別④モジュール構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: Copro AI(Mihalyデータ込み、高G旋回支援)、
  PLSL(Pulse Laser、機関砲スロット代替。AC7 では F-15C / MiG-31B / Su-57 にも
  special weapon pod として搭載可能、射程約5000m、雲で減衰)、支援UAVランチャー
  (MDのドローン射出モジュール群と整合確認要)。
  ノーズユニット分離・有人/無人切替は HOI4 の通常航空機モデルでは
  直接表現困難な可能性あり(要判断)。

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
- **HOI4実装判定**: × 除外(機体本体) / 要判断
- **判定根拠**: 機体本体追加は本MOD方針外。ただし本機は「群運用UAV」であり、
  種別④で「ドローン群モジュール」(MDの既存 `weap_droneswarm_fighter_1` 系の
  バリエーション)として再定義可能な可能性あり。その場合は種別④で
  ○ モジュール化可 として扱う。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機自体はプレイヤー使用不可の敵UAV。MDのアーセナルバード関連
  ドローンスワームモジュール群が直接対応する候補。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(衛星リモートCOFFIN、UAVネットワーク制御)は種別④
  モジュール構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: 衛星リモートCOFFIN、MQ-90L Quox bis との
  UAVネットワーク制御(レーザCIWS含む)。武装はCFA-44と共通(別エントリ参照)。

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
- **HOI4実装判定**: × 除外(機体本体) / 要判断
- **判定根拠**: 機体本体追加は本MOD方針外。ただし量産UAVであり、種別④で
  「UAVモジュール」(MQ-99と同じく既存ドローン枠の拡張)として
  再定義可能な可能性あり。その場合は種別④で ○ モジュール化可。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: ACI設定では制式採用UAV(2010年代に米国等へ輸出)。
  MQ-99(攻撃用)とは異なるW翼レイアウトが特徴。

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
- **HOI4実装判定**: × 除外(機体本体)
- **判定根拠**: 機体本体追加は本MOD方針外(本セクション共通方針)。
  特徴的システム(視線+音声制御COFFIN、機首MPBM)は種別④モジュール
  構成要素で別エントリ化対象。
- **MD既存実装との対応**: 未確認(MD実コード未取得)
- **備考**: 本機由来のモジュール候補: 視線+音声制御COFFIN(ADF-01の
  伏臥型COFFINとは別系統)、機首MPBM。ACI 限定機。

---

## 種別 ③ 架空艦

本セクションは AC作品に登場する in-universe の架空艦船を扱う。

### 設計方針(2026-06-28 本人確定)

- **シンファクシ級・アリコーン** → 新規アーキタイプ
  **「超大型潜水艦船体」(仮称: `super_submarine_equipment`)** を追加し、
  その variant として両艦を実装。各艦の差別化はモジュール選択で行う。
- **アイガイオン級空中艦・グレイプニル・アーセナルバード・アークバード** →
  MD既存の `mothership_equipment` をテンプレにアーキタイプ + variant + モジュールを
  追加(超兵器セクションで既決)。**最悪、新variant追加せず既存
  アーセナルバード本体に追加モジュールだけ載せる妥協案**もアリ。
- **通常艦・在来型**(Hubert級空母、Leasath 潜水艦 Proteus/Triton、
  Aegir Fleet)→ 除外
- **超兵器の艦載兵器** → **MD既存と非競合**のもののみ種別④に追加

### 対象艦の振り分け

| 対象艦 | 取り扱い | 参照 |
|--------|----------|------|
| シンファクシ級 (Scinfaxi & Hrimfaxi) | 新規「超大型潜水艦」アーキタイプの variant | 種別①/種別③詳細 |
| アリコーン (Alicorn) | 同上(シンファクシ級と共通アーキタイプ) | 種別①/種別③詳細 |
| アイガイオン級空中艦・グレイプニル・アーセナルバード・アークバード | `mothership_equipment` 流用(variant追加 or 既存に追加モジュール) | 種別① |
| Hubert級空母 (OFS Kestrel 等)・Leasath 潜水艦 Proteus / Triton・Aegir Fleet | × 除外(在来型、本MOD方針外) | — |

### 超大型潜水艦アーキタイプ(設計案)

MD既存に超大型潜水艦の枠は無い(`attack_submarine` / `missile_submarine` は
通常規模)。本MODでは航空機の `mothership_equipment`(line 6086)を参考に、
艦版の専用アーキタイプを新規追加する。

想定スロット構成(`mothership_equipment` パターンの艦版翻案):
- `fixed_primary_command_operations`(必須)
- `super_sub_main_weapon_slot`(主砲枠、必須) — 例: アリコーンの SRC-03a 600mm
- `super_sub_auxiliary_slot_1〜4`(補助武装枠) — 例: SLUAVベイ、SOFFM、艦上ヘリパッド、VLS等
- `super_sub_propulsion_slot`(必須、原子力前提)
- `super_sub_avionics_slot`(必須、潜水艦用ソナー/レーダ)
- `super_sub_stealth_slot`(任意、ステルス材料)
- `super_sub_flight_deck_slot`(任意、艦載機運用が必要な場合のみ)

variant 差別化案:
- **variant: シンファクシ級** = 主砲なし / 弾道ミサイル発射ポート + SOFFM対空 + V/STOL運用甲板
- **variant: アリコーン** = SRC-03a 主砲 + SLUAVベイ + 大型滑走路(MD既存flight_deck) + 多数VLS(MD既存)

### 種別③ 由来の艦載モジュール候補(MD既存非競合のみ採用、3件)

| モジュール | 由来艦 | MD既存との関係 | 種別④ |
|------------|--------|----------------|--------|
| **SRC-03a 超大型レールキャノン**(600mm、核砲弾) | アリコーン | `module_railguns_category` の桁外れ上位tier | エントリ化済み |
| **対空バースト弾頭ミサイル (SOFFM-type)** | シンファクシ級 | 既存対空兵装とは burst warhead 性質が独自 | エントリ化済み |
| **SLUAV 発射ベイ**(潜水艦から空中UAVを射出) | アリコーン | `module_submarine_drone_control` は水中ドローン制御で別物 | エントリ化済み |

### 除外項目(MD既存と直接競合)

| 項目 | MD既存カテゴリ | 対応 |
|------|----------------|------|
| アリコーン 200mm 補助レールガン | `module_railguns_category` | MD既存の通常 railguns で代用 |
| アリコーン 48基VLS | `module_vls_category` / `module_vls_sub_category` | MD既存 VLS で代用 |
| アリコーン 艦上滑走路 | `module_flight_decks_category` | MD既存 flight deck で代用 |
| シンファクシ級 弾道ミサイル発射ポート | `module_vls_sub_category` | MD既存 VLS_sub で代用 |
| シンファクシ級 二重ステルス材料 | MD既存ステルス枠 | MD既存ステルスで代用 |

### 統計

- 種別③本体エントリ: 0(超兵器セクション側で扱う構造のため)
- 種別③由来の艦載モジュール(種別④エントリ化済み): 3 件

---

## 種別 ④ モジュール構成要素(候補サマリ)

本セクションは、種別②架空機エントリの末尾「本機由来のモジュール候補」を
集約し、本MODが追加する **モジュール候補** をリスト化したものである。
個別エントリ(典拠URL・MD既存対応・○判定根拠)は、Steam β版MD
(`...\workshop\content\394360\3374271790\`)で各モジュールのスロット可否を
確認した後に順次追記する。

### 確定モジュール候補(14個)

(2026-06-28 本人確定。F.防御系・MSTM・Dark Fire/Star Fireは方針整理で除外)

| 区分 | モジュール | tier | 想定スロット | 出典機体 |
|------|------------|------|--------------|----------|
| A | COFFINコックピット | 1 | コックピット枠 | ADF-01 FALKEN(他バリエーション統合) |
| B | TLS(機載レーザ砲) | 2 | 特殊兵装枠 | ADFX-01 → ADFX-02 |
| B | HPM(対エンジンマイクロ波) | 1 | 特殊兵装枠 | XFA-33 Fenrir |
| C | 機載レールガン | 2 | 特殊兵装枠 | CFA-44 EML → X-02S Arclight |
| D | MPBM(空中炸裂) | 1 | ミサイル枠 | ADFX-01/02、ADA-01B |
| D | 多目的全方位ミサイル | 2 | ミサイル枠 | XFA-24A ODMM → CFA-44 ADMM |
| D | LSWM(超長射程衝撃波) | 1 | ミサイル枠 | XFA-33 Fenrir、GAF-1 Varcolac |
| E | ベルカ系ECM | 2 | ECM/アビオ枠 | ADFX-01 Morganite → ADFX-02 全方位 |
| E | 攻撃型ECM "Cocytus" | 1 | ECM/アビオ枠 | CFA-44 Nosferatu |
| G | 子機搭載ドローン群 | 2 | `plane_droneswarm_weapon`(母機強化として抽象化) | CFA-44 UAV-45s、ADF-11F 支援UAV |
| G | UAVネットワーク制御 | 1 | `plane_drone_systems`(母機強化として抽象化) | QFA-44 + MQ-90L Quox bis |
| H+I | 機載AI支援システム | 1 | コンピュータ/アビオ枠 | ADF-11F Copro AI + XFA-24A Earth Shaker(統合) |
| J | PLSL(Pulse Laser) | 1 | 機関砲枠(機銃代替) | ADF-11F Raven / DarkStar |

### 集計

- 区分 A:1 / B:2 / C:1 / D:3 / E:2 / G:2 / H+I:1 / J:1 = **計13モジュール**
- F.防御系(APS / 光学迷彩 / レーザCIWS)は MD既存実装で対応可のため候補から除外
- G.群運用攻撃UAV(MQ-99系)は 2026-06-28 本人指摘で除外
  (MDの droneswarm_weapon は実体ドローン射出ではなく母機ステータス強化に
  すぎず、MQ-99のような「独立飛行UAV」をモジュール化できないため。
  MQ-99 機体本体は種別②で `× 除外` 済み)

### 検討除外項目(参考)

| 項目 | 除外理由 |
|------|----------|
| MSTM(短時間連射ミサイル) | MD既存ミサイルtierで充足 |
| Dark Fire AAM / Star Fire 対艦ミサイル | 通常ミサイル枠の改良として既存装備にぶら下げる扱い |
| F.防御系 全般 | MD既存実装で対応可 |
| 多視点センサ配列(ADF-01 FALKEN由来) | 単独モジュール化価値が低い(COFFINに吸収) |
| VTOL / CATOVL | HOI4の通常航空機モデルでは表現困難の可能性。要判断 |
| 群運用攻撃UAV(MQ-99系) | 2026-06-28 本人指摘 — MDの `plane_droneswarm_weapon` は実体ドローン射出ではなく母機ステータス強化、独立飛行UAVをモジュール化不可。MQ-99 機体本体は種別②で `× 除外` 済み |

### MD既存カテゴリと本MOD候補のマッピング(2026-06-28 β版で確認済み)

下表は Steam β版 (`...\3374271790\common\units\equipment\modules\MD_plane_modules.txt`)
を直接読んで確認した、MDの既存モジュールカテゴリ。本MODの14候補はすべて
**既存カテゴリの allowed_module_categories に新モジュール定義を追加する**
だけで実装可能で、新規カテゴリ追加は不要(全件 `○ モジュール化可`)。

| MD既存カテゴリ | 既存定義の場所 | 本MOD候補 |
|----------------|----------------|-----------|
| `plane_multipurpose_gun` | MD_plane_modules.txt:1978〜 | PLSL |
| `plane_avionics` | MD_plane_modules.txt:1283〜 | COFFINコックピット、機載AI支援 |
| `plane_countermeasures` | MD_plane_modules.txt:7985〜 | ベルカ系ECM、攻撃型ECM Cocytus |
| `plane_heavy_special_design_arsenal` | MD_plane_modules.txt:7647 | TLS、HPM、機載レールガン |
| `plane_fighter_weapons` / `plane_stealth_mr_weapons` | MD_plane_modules.txt:2125〜 | MPBM、多目的全方位ミサイル、LSWM |
| `plane_droneswarm_weapon` | MD_plane_modules.txt:8211〜8329 | 子機搭載ドローン群(母機ステータス強化として抽象化) |
| `plane_drone_systems` | MD_plane_modules.txt:1476〜 | UAVネットワーク制御 |

アーセナルバード(`mothership_equipment` line 6086)は `plane_heavy_special_design_arsenal`
を `special_slot_type_2` で受け入れている(`MD_plane_airframes.txt:6190-6195`)。
このパターンを **通常戦闘機のスロット拡張** にも適用するのが本MODの実装方針。

### 各モジュール個別エントリ

---

#### COFFINコックピット (COFFIN Cockpit)

- **種別**: モジュール構成要素
- **登場作品**: AC2(ADF-01 FALKEN 初出)、ACX(XFA-33 Fenrir、限定版はXFA-27)、
  AC7(QFA-44 Carmilla 衛星リモート版)、ACI(ADA-01B ADLER 視線+音声制御版)
- **製造主体(in-universe)**: 各機により異なる(オーシア Gründer Industries、
  リーサス、他)
- **性質**: パイロットの操縦インターフェース。伏臥姿勢で機体周囲の多視点
  センサ・360度ディスプレイから視認、または視線+音声で操縦する。
  本MODでは仕様差はモジュール tier ではなく1モジュールに統合し、効果は
  「パイロット負担軽減」「機動性向上」「視界拡張」の汎用ボーナスとする。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADF-01_FALKEN
  - https://acecombat.wiki.gg/wiki/QFA-44_Carmilla
  - https://acecombat.wiki.gg/wiki/ADA-01B_ADLER
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_avionics` カテゴリに新モジュールとして追加。
  既存スロット `avionics_type_slot` の allowed_module_categories は `plane_avionics`
  のみのため、本MODの新モジュールは既存スロットへそのまま割り当て可能。
- **MD既存実装との対応**: 該当カテゴリあり。
  既存例: `plane_avionics` の最初の定義群が
  `MD_plane_modules.txt:1283-1471` 付近に複数tier並ぶ。
- **備考**: 視線+音声制御 / 衛星リモート版は別 tier 化候補だが、第一段階では
  単一モジュール。後続で sub-variant 化する余地あり。

---

#### TLS (Tactical Laser System / 機載レーザ砲)

- **種別**: モジュール構成要素
- **登場作品**: ACZ(ADFX-01 Morgan 初出、TLS "Zoisite")、ACZ(ADFX-02 Morgan
  指向性版)、AC2 再登場(ADF-01 FALKEN)
- **製造主体(in-universe)**: South Belka Munitions Factory(後の Gründer Industries)
- **性質**: 機載化学レーザ砲。エンジン間または機首に搭載。基本版は固定射線で
  7G制限あり、上位版は油圧+照準系で指向性制御可能。本MODでは
  2 tier(基礎: Zoisite相当 / 改良: 指向性相当)。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Tactical_Laser_System
  - https://acecombat.wiki.gg/wiki/ADFX-01_Morgan
  - https://acecombat.wiki.gg/wiki/ADFX-02_Morgan
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_heavy_special_design_arsenal` カテゴリ
  (アーセナルバード用重特殊兵装枠)に新モジュールとして追加。
- **MD既存実装との対応**: 該当カテゴリあり。既存定義: `MD_plane_modules.txt:7647`。
  アーセナルバードでは `special_slot_type_2` で受入(`MD_plane_airframes.txt:6190-6195`)。
  通常戦闘機への追加は新規スロット定義または既存特殊兵装スロットの
  allowed_module_categories 拡張で実現。
- **備考**: 2 tier。ベルカ系技術ツリーの基幹モジュール。
  ADF-01 FALKEN は ADFX-02 残骸由来のため同系統技術として備考に明記。

---

#### HPM (High-Powered Microwave / 対エンジンマイクロ波)

- **種別**: モジュール構成要素
- **登場作品**: ACX(XFA-33 Fenrir 初出)
- **製造主体(in-universe)**: Leasath Air Force New Weapon Laboratory(リーサス連邦)
- **性質**: 交差マイクロ波ビームで敵機エンジンを過熱・無力化する実験兵器。
  通常の指向性エネルギー兵器(レーザ)と違い、命中目標を機構的に「壊す」
  のではなく「動作不能化」する。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XFA-33_Fenrir
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: TLSと同じく `plane_heavy_special_design_arsenal` カテゴリ
  への新モジュール追加で実装。
- **MD既存実装との対応**: 該当カテゴリあり(`MD_plane_modules.txt:7647`)。
  TLSと同じスロットを共有。
- **備考**: TLSと差別化するため、効果を「敵機機動性低下」または
  「迎撃命中率ボーナス」とする想定(物理ダメージではなく機能無力化)。

---

#### 機載レールガン (Aircraft-Mounted Railgun)

- **種別**: モジュール構成要素
- **登場作品**: AC6(CFA-44 Nosferatu の EML "Purgatorio" 初出)、
  AC7(X-02S Strike Wyvern の Arclight 電磁射出機)
- **製造主体(in-universe)**: Albastru-Electrice(エストバキア、CFA-44)、
  EASA + Gründer Industries(エルジア+オーシア、X-02S)
- **性質**: 機載電磁射出機。実弾を超高速で投射する。CFA-44のEMLは連射式、
  X-02SのArclightは中央兵装ベイ搭載で大型化。本MODでは2 tier
  (基礎: EML相当 / 改良: Arclight相当)。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/CFA-44_Nosferatu
  - https://acecombat.wiki.gg/wiki/X-02S_Strike_Wyvern
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: TLSと同じく `plane_heavy_special_design_arsenal` カテゴリ
  への新モジュール追加で実装。
- **MD既存実装との対応**: 該当カテゴリあり(`MD_plane_modules.txt:7647`)。
- **備考**: アリコーンの艦載大型レールキャノン(超兵器セクション)とは別物。
  本モジュールは機載小型版。

---

#### MPBM (Multi-Purpose Burst Missile / 空中炸裂ミサイル)

- **種別**: モジュール構成要素
- **登場作品**: ACZ(ADFX-01/02 Morgan)、ACI(ADA-01B ADLER 機首版)
- **製造主体(in-universe)**: South Belka Munitions Factory
- **性質**: 空中炸裂式の多目的ミサイル。広範囲攻撃に特化し、対群目標で
  有効。ADA-01B は機首発射口に小型化して搭載(機動性確保)。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADFX-01_Morgan
  - https://acecombat.wiki.gg/wiki/ADA-01B_ADLER
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_fighter_weapons` または `plane_stealth_mr_weapons`
  カテゴリに新モジュールとして追加。
- **MD既存実装との対応**: 該当カテゴリあり。
  `plane_fighter_weapons` 既存定義群: `MD_plane_modules.txt:2125〜`。
  `plane_stealth_mr_weapons` は `:2450〜`。
- **備考**: 対群効果(密集編隊への複数命中ボーナス等)で差別化。

---

#### 多目的全方位ミサイル (Multi-direction Missile / ODMM-ADMM 系)

- **種別**: モジュール構成要素
- **登場作品**: ACX(XFA-24A Apalis の ODMM 初出)、AC6(CFA-44 Nosferatu の
  ADMM "Inferno"、12機同時ロック)
- **製造主体(in-universe)**: 不明(Apalis)、Albastru-Electrice(エストバキア、CFA-44)
- **性質**: 全方位に発射可能な多目的ミサイル。複数目標同時ロックが特徴。
  本MODでは2 tier(基礎: ODMM相当 / 改良: ADMM相当 = 同時ロック数大幅増)。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/CFA-44_Nosferatu
  - https://acecombat.wiki.gg/wiki/XFA-24A_Apalis
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: `plane_fighter_weapons` または `plane_stealth_mr_weapons`
  カテゴリに新モジュールとして追加。
- **MD既存実装との対応**: 該当カテゴリあり(MPBMと同じカテゴリ群)。
- **備考**: 同時複数交戦効果(命中数ボーナス、SAM飽和耐性等)で差別化。

---

#### LSWM (Long-range Shock Wave Missile / 超長射程衝撃波ミサイル)

- **種別**: モジュール構成要素
- **登場作品**: ACX(XFA-33 Fenrir、グレイプニル弾道弾の縮小版として初出)、
  ACJA(GAF-1 Varcolac 互換)
- **製造主体(in-universe)**: Leasath(初出機)、不明(Varcolac)
- **性質**: 超長射程の衝撃波ミサイル。グレイプニル(超兵器)由来の縮小版で、
  長距離砲撃を機載化したもの。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/XFA-33_Fenrir
  - https://acecombat.wiki.gg/wiki/GAF-1_Varcolac
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: `plane_fighter_weapons` または `plane_heavy_nav_weapons`
  (重対艦兵装)カテゴリに新モジュールとして追加。
- **MD既存実装との対応**: 該当カテゴリあり。
  `plane_heavy_nav_weapons` 既存定義: `MD_plane_modules.txt:6277〜`。
- **備考**: 射程・威力で差別化。重対艦兵装カテゴリも候補(用途次第)。

---

#### ベルカ系ECM (Belkan ECM Suite / Morganite → 全方位)

- **種別**: モジュール構成要素
- **登場作品**: ACZ(ADFX-01 Morgan の Morganite ECM 初出)、ACZ(ADFX-02 Morgan の
  全方位ECM、正面以外全方位の弾道偏向)
- **製造主体(in-universe)**: South Belka Munitions Factory
- **性質**: 電子戦ポッド。基礎版は敵ミサイル誘導阻害+味方ミサイル強化。
  上位版は正面以外の全方位からのミサイル・弾丸を偏向する。
  本MODでは2 tier(基礎: Morganite / 改良: 全方位)。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADFX-01_Morgan
  - https://acecombat.wiki.gg/wiki/ADFX-02_Morgan
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_countermeasures` カテゴリに新モジュール
  として追加。
- **MD既存実装との対応**: 該当カテゴリあり。
  既存定義: `MD_plane_modules.txt:7985〜`。
- **備考**: ベルカ系技術ツリーの主要モジュール群の一つ。

---

#### 攻撃型ECM "Cocytus" (Offensive ECM with Sensor-Burn Beam)

- **種別**: モジュール構成要素
- **登場作品**: AC6(CFA-44 Nosferatu)
- **製造主体(in-universe)**: Albastru-Electrice(エストバキア)
- **性質**: 通常のECM機能に加え、センサ焼き切りビーム(攻撃用途)を備えた
  攻撃的電子戦ポッド。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/CFA-44_Nosferatu
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: `plane_countermeasures` カテゴリに新モジュールとして追加。
  ベルカ系ECMと同じカテゴリだが、効果が「防御」ではなく「攻撃」のため
  別モジュール化。
- **MD既存実装との対応**: 該当カテゴリあり(`MD_plane_modules.txt:7985〜`)。
- **備考**: 効果は「敵機センサ無効化」「対SAM攻撃力ボーナス」等で差別化想定。

---

#### 子機搭載ドローン群 (Sub-drone Swarm Module)

- **種別**: モジュール構成要素
- **登場作品**: AC6(CFA-44 Nosferatu の UAV-45s "Malebolge")、AC7(ADF-11F Raven の
  支援UAV、翼下から最大2基射出)
- **製造主体(in-universe)**: Albastru-Electrice(CFA-44 UAV-45s)、
  Gründer Industries + EASA(ADF-11F)
- **性質**: 設定上は親機(母機)に搭載する小型自律ドローン群で、空戦中に
  射出してロック支援・追撃・攻撃を自律で行う。本MODでは2 tier想定。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/CFA-44_Nosferatu
  - https://acecombat.wiki.gg/wiki/ADF-11F_Raven
- **HOI4実装判定**: ○ モジュール化可(母機ステータス強化として抽象化)
- **判定根拠**: MD既存の `plane_droneswarm_weapon` カテゴリに新モジュール
  として追加。アーセナルバードの `weap_droneswarm_fighter_1` 系と同パターン。
- **MD既存実装との対応**: 該当カテゴリあり。
  既存定義: `MD_plane_modules.txt:8211-8329` 付近に4つの既存
  droneswarm_weapon 例(`weap_droneswarm_fighter_1` / `_bomber_1` /
  `weap_missile_bay_1` / `_anti_ship_1`)。**MD実装は実体ドローンを
  飛ばすのではなく `add_stats` で母機のair_superiority等を強化する形式**
  (例: `weap_droneswarm_fighter_1` は air_attack +35 / air_agility +50)。
- **備考**: **重要 — 設定と実装のギャップ**: AC本編の UAV-45s や ADF-11F
  支援UAV は「実際に子機を射出して群を運用」する設定だが、MDの
  `plane_droneswarm_weapon` は「母機のステータスを強化するモジュール」
  という抽象化で実装されている。本MODもこの抽象化を踏襲する
  (HOI4 では母機-子機関係を直接表現する仕組みがないため)。
  設定考証として「子機を本当に飛ばすわけではない」を備考扱いとする。
- **設計方針(2026-06-28 本人確認)**: アーセナルバード専用カテゴリだった
  `weap_droneswarm_fighter_1` 系をテンプレに、本MODでは数値・xp_cost・
  resourcesを通常戦闘機向けに調整(下方修正)した派生tier として実装する。
  既存値(air_attack +35 / air_agility +50 等)はアーセナルバード前提の
  豪華設定のため、戦闘機ベースでは過剰になる可能性が高い。

---

#### UAVネットワーク制御 (UAV Network Control Interface)

- **種別**: モジュール構成要素
- **登場作品**: ACI(QFA-44 Carmilla + MQ-90L Quox bis 連携)
- **製造主体(in-universe)**: 不明(QFA-44 製造主体)
- **性質**: 母機側のUAV群統合制御用の通信・指揮システム。設定上は独立飛行する
  UAV群(MQ-90L Quox bis 等)を母機側からネットワーク制御し、レーザCIWS的な
  防御運用も可能とする。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/QFA-44_Carmilla
- **HOI4実装判定**: ○ モジュール化可(母機側の指揮・連携能力強化として抽象化)
- **判定根拠**: MD既存の `plane_drone_systems` カテゴリに新モジュール
  として追加。MD実装上は母機側のステータスを強化するモジュール。
- **MD既存実装との対応**: 該当カテゴリあり。
  既存定義: `MD_plane_modules.txt:1476〜`(12個の既存ドローンシステム例あり)。
- **備考**: 「子機搭載ドローン群」と同様、HOI4 のメカ的制約から「独立UAV群を
  母機が指揮する」設定は母機側ステータス強化で抽象化される。

---

#### (削除) 群運用攻撃UAV (MQ-99系)

2026-06-28 本人指摘により本エントリは削除。

- **削除理由**: MQ-99 系は「母機から射出する子機ドローン群」ではなく、
  「コンテナ射出されてからは独立飛行する単独完結UAV」であり、
  MD の有人機と本質的に同じ「独立アーキタイプを要する航空機」である。
  種別④モジュールとしての扱いは不適切。
- **取り扱い**: MQ-99 単体は既に **種別②架空機** に記載済み
  (判定 `× 除外(機体本体) / 要判断`)で完結。実装するなら新規アーキタイプ
  追加が必要となるが、本MOD方針(機体本体追加は方針外)により実装ターゲット外。
- **MD側の前例**: MD の `plane_droneswarm_weapon` は実体ドローンの射出ではなく
  母機ステータス強化モジュールであることを確認(2026-06-28 β版実コード再読)。

---

#### 機載AI支援システム (Onboard AI Assist System)

- **種別**: モジュール構成要素
- **登場作品**: AC7(ADF-11F Raven の Copro AI、Mihaly A. Shilage の戦闘
  データロード)、ACX(XFA-24A Apalis の Earth Shaker、地形デジタル解析)
- **製造主体(in-universe)**: Gründer Industries + EASA(Copro)、不明(Earth Shaker)
- **性質**: 機載コンピュータの支援AI。「エースパイロットデータロード」
  「高G旋回支援」「地形解析による対地攻撃強化」など、パイロットの
  操作を計算機側が補佐する。本MODでは1モジュールに統合し、複合効果
  (機動ボーナス + 対地命中ボーナス)を持つ。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/ADF-11F_Raven
  - https://acecombat.wiki.gg/wiki/XFA-24A_Apalis
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_avionics` カテゴリに新モジュールとして追加。
  既存スロット `avionics_type_slot` 経由で実装可能。
- **MD既存実装との対応**: 該当カテゴリあり(`MD_plane_modules.txt:1283〜`)。
  COFFINコックピットと同じカテゴリだが、効果(操縦支援 vs パイロット負担軽減)
  が違うため別モジュール化。
- **備考**: 第一段階では複合効果単体モジュール。第二段階で対空寄り(Copro)・
  対地寄り(Earth Shaker)のsub-variant化を検討。

---

#### PLSL (Pulse Laser / 機関砲スロット代替)

- **種別**: モジュール構成要素
- **登場作品**: AC7(ADF-11F Raven / DarkStar の標準装備、F-15C / MiG-31B /
  Su-57 にも special weapon pod として搭載可能)
- **製造主体(in-universe)**: 不明(複数機種が標準/オプション搭載)
- **性質**: 機関砲スロットを置き換える短時間バースト発射型レーザ。射程約
  5000m、雲で減衰の特性付き。機関砲との互換性で同スロットに装着。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Pulse_Laser
  - https://acecombat.wiki.gg/wiki/ADF-11F_Raven
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `plane_multipurpose_gun`(機関砲枠)カテゴリに
  新モジュールとして追加。
- **MD既存実装との対応**: 該当カテゴリあり。
  既存定義: `MD_plane_modules.txt:1978〜`(複数tierの機関砲モジュール例あり)。
- **備考**: 「機銃の代替」=同スロット占有のため、独立カテゴリ追加不要で
  既存スロットにそのまま入る最も実装が軽いモジュール候補。
  雲減衰の表現は「天候不良時の命中率ペナルティ」で簡易再現可能。

---

### 艦載モジュール候補(超大型潜水艦アーキタイプ向け、種別③由来)

以下3件は **MD既存艦載モジュールと非競合**な独自モジュールとして追加する。
種別③の「超大型潜水艦」アーキタイプ(シンファクシ級・アリコーン variant)の
スロットに割り当てて差別化に使う想定。MD既存と競合する候補(200mm補助レールガン、
48基VLS、艦上滑走路、弾道ミサイル発射ポート、二重ステルス材料)はMDの
既存モジュールを最大tier割り当てで代用し、独自モジュール化はしない。

---

#### SRC-03a 超大型レールキャノン (Super-Heavy Naval Railcannon)

- **種別**: モジュール構成要素(艦載)
- **登場作品**: AC7(アリコーン搭載)
- **製造主体(in-universe)**: ユークトバニア(建造)→ エルジア(運用)
- **性質**: 600mm口径、76.5m砲身の超大型レールキャノン。核砲弾の発射が可能。
  通常のレールガン(MD既存 `module_railguns_category` の上限である艦載
  補助砲規模)から完全に逸脱した「主砲」級の威力・サイズ。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Alicorn
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `module_railguns_category` (または
  `module_railgun_battery_category`)の**桁外れ上位tier**として新モジュール
  追加。同カテゴリの最高tierと比較しても性能・コスト・xp_costを大幅に上回る
  「超大型主砲」枠として差別化。
- **MD既存実装との対応**: 該当カテゴリあり。
  `module_railguns_category` 既存定義: `MD_ship_modules.txt:4067-4286` 付近に
  6tier。`module_railgun_battery_category` は `:4286-4453` 付近に4tier。
  本MODの追加tierはこれら既存tierの上に位置づける。
- **備考**: 核砲弾発射可能の表現は、build_cost に uranium 消費を追加 or
  特殊兵装フラグで再現。MDの核武装関連(`plane_nuclear_consent`等)と整合させる。

---

#### 対空バースト弾頭ミサイル (SOFFM-type Anti-Air Burst Warhead Missile)

- **種別**: モジュール構成要素(艦載)
- **登場作品**: AC5(シンファクシ級搭載)
- **製造主体(in-universe)**: ユークトバニア国営兵器工場
- **性質**: 大型潜水艦から発射される対空バースト弾頭ミサイル。命中前に
  炸裂し広範囲に拡散弾頭を放つことで、編隊規模の航空機群を一掃する
  ことを狙う対空兵装。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Scinfaxi-class_submarine
- **HOI4実装判定**: ○ モジュール化可
- **判定根拠**: MD既存の `module_vls_sub_category` (潜水艦VLS)に新モジュール
  として追加。MDの既存対空兵装は通常規模の SAM 系で、burst warhead 拡散
  弾頭性質は独自。
- **MD既存実装との対応**: 該当カテゴリあり。
  `module_vls_sub_category` 既存定義: `MD_ship_modules.txt:5256-5658` 付近に
  15tier。本MODのSOFFM-typeは「対航空編隊特化」として既存通常VLS_subと差別化
  (air_attack ボーナス、anti-air 命中数ボーナス等)。
- **備考**: シンファクシ級独自のアイデンティティとなる対空兵器。
  通常VLSと同スロット競合のため、選択して載せる形になる。

---

#### SLUAV 発射ベイ (Submarine-Launched UAV Bay)

- **種別**: モジュール構成要素(艦載)
- **登場作品**: AC7(アリコーン搭載、16基)
- **製造主体(in-universe)**: エルジア
- **性質**: 潜水艦から空中無人機(SLUAV = Submarine-Launched UAV)+
  バリアドローンを射出するベイ。母艦(潜水艦)を中心に空中UAV防衛網を
  展開し、航空攻撃を妨害する。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Alicorn
- **HOI4実装判定**: ○ モジュール化可(母艦ステータス強化として抽象化)
- **判定根拠**: MDには「潜水艦が空中UAVを射出する」モジュールカテゴリは
  既存しない(`module_submarine_drone_control` は水中ドローン制御で別物)。
  独立カテゴリ追加または既存 `module_submarine_drone_control` の派生として
  「空中UAV制御」を追加可能。航空機側の `plane_droneswarm_weapon` 同様に
  「母艦のair_attack/air_defence/anti_air_attack を強化するモジュール」として
  抽象化実装する。
- **MD既存実装との対応**: 直接対応カテゴリは無いが、隣接カテゴリあり。
  `module_submarine_drone_control` 既存定義: `MD_ship_modules.txt:2940`。
  本MODの「SLUAV発射ベイ」はこのカテゴリの空中版として新カテゴリ追加
  (`module_super_sub_air_drone_bay_category` 等)。
- **備考**: ドローン群モジュール(種別④航空機モジュール)と同じく、
  「実体UAVを飛ばすのではなく母艦ステータス強化として抽象化」する方針。

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

(2026-06-28 再々更新: 種別④モジュール構成要素 14件を個別エントリ化完了。
MD β版でカテゴリ確認済みのため全件 `○ モジュール化可` + MD対応「対応物あり」。
本MODの実装ターゲットが具体化した。)

- エントリ総数: 49
- 種別別件数:
  - 超兵器: 17
  - 架空機: 16
  - 架空艦: 0(超兵器側 + 種別③で扱う構造のため)
  - モジュール構成要素: 16(航空機13件 + 艦載3件)
  - 実在機: 0
- 判定別件数:
  - ○ モジュール化可: 16(航空機モジュール13件 + 艦載モジュール3件)
  - △ アーキタイプ追加: 9(超兵器セクションの空中艦・空母系)
  - × 除外: 24(超兵器固定設置型 8件 + 架空機本体 16件)
  - 要判断: 0
- MD対応状況:
  - 対応物あり: 23(超兵器7件 + 種別④16件)
  - 該当なし: 0
  - 未確認: 26(超兵器10件 + 架空機16件)
- 要追加調査リストの件数: 18(超兵器10件 + 架空機未エントリ化8件、確認済み7件を除く)
