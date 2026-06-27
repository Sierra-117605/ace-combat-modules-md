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

### 1.2 ファイル構成(暫定、要調査)

HOI4 mod の標準ディレクトリ構造に従う:

- `common/units/equipment/modules/` — 新規モジュール定義
- `common/special_projects/projects/` — 新規 Special Project(該当する場合)
- `common/technologies/` — 解放用技術(該当する場合)
- `gfx/interface/` — モジュールアイコン
- `interface/` — gfx登録
- `localisation/english/` — 英語ローカライゼーション
- `localisation/japanese/` — 日本語ローカライゼーション(対応する場合)

具体的なパスはMD既存ファイルの命名規則を Claude Code / Codex で
精査してから確定する。

### 1.3 命名規則(暫定、要決定)

- モジュールID接頭辞: `acm_` を予定(ACM-MDの略)
- 既存MDモジュールと衝突しないことを `grep` で確認する
- 例: `acm_railgun_main_battery`, `acm_railgun_secondary`, `acm_sluav_bay`

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

## 3. モジュール定義の技術仕様(要調査)

MDがどのようにモジュールを定義しているかを、Claude Code / Codex 側で
精査してから確定する。要調査項目:

- モジュールカテゴリ(`category`)の定義方法
- 既存スロットに新カテゴリを許可する方法
  (`allowed_module_categories` への追加)
- モジュールの性能修正値(modifier)の書式と利用可能な statキー一覧
- 各アーキタイプのスロット構成と、どこに新カテゴリを差し込めるか
- アイコン .dds のサイズ・圧縮形式
- `.gfx` ファイルへのアイコン登録方法
- ローカライズキーの命名規則
- Special Projects への新規プロジェクト追加手順
- MD独自の改変点(バニラとの差分)

これらは Claude Code/Codex に調査タスクとして渡し、結果を
KNOWLEDGE.md に蓄積する。

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

### 4.3 第一段階で実装する兵器(未決定)

DATABASE.md の判定結果を踏まえ、最小スコープで1〜2モジュールから着手。
最初の実装対象は以下の条件を満たすものを優先する:

- HOI4実装判定が `○ モジュール化可`
- MD既存実装と機能が被らない
- アイコンが既存流用または最小限の新規作成で済む
- 効果が分かりやすく、動作確認しやすい

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

### 6.3 HOI4本体DLCとの依存(要調査)

- MD自体が NSB / MtG / BBA / By Blood Alone 等のDLCに依存している
- 本MODはMDの依存に追従するため、独自のDLC依存は持たない予定
- Special Projects を使う場合は By Blood Alone が前提

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
