# DATABASE.md 作成指示書

本書は、HOI4 mod「Millennium Dawn」向けのACモジュールMODに使用する
ローカル兵器データベース DATABASE.md を作成するための指示書である。
Claude Code または Codex 等のコーディングエージェントは本書の内容を厳守すること。

---

## 1. 目的

エースコンバット由来の超兵器・架空兵器、ならびに本MODで取り扱う実在兵器の
ローカルデータベース DATABASE.md を作成する。

用途:
- ACモジュールMODのモジュール追加候補の判定
- 実装難度の見積もり
- 会話セッション間で根拠を保持し、ハルシネーション(推測情報の混入)を防止する

本DBはMOD実装の「典拠台帳」として機能する。
ここに無い情報を MOD実装に使うことは禁止する。

---

## 2. 出力ファイル

- 出力先パス: リポジトリのルート直下に `DATABASE.md`
- フォーマット: Markdown (GitHub Flavored Markdown)
- 文字コード: UTF-8
- 言語: 日本語をメインとする
- 固有名詞は「日本語(英語原語)」併記
  - 例: 「アリコーン (Alicorn)」「ストーンヘンジ (Stonehenge)」
- 最終的に HTML 版へ変換する想定。本タスクでは .md のみ作成し、HTML化は別タスク

---

## 3. 一次情報源(典拠の優先順位)

優先度の高い順に当たること。下位ソースを使うのは上位で確認できなかった場合のみ。

### 超兵器分類(例外ルール)

**公式(Project Aces, acecombat.jp の ACES CHRONICLE)を最優先**とする:

1. **acecombat.jp/en/chronicle/weapons/** (公式 ACES CHRONICLE)
2. acecombat.wiki.gg (ただし Disputed セクションの独自判定は採用しない)

理由: 公式は2025年11月更新でaerial warships(XB-0, Sphyrna, Aigaion,
Kottos, Gyges, Gleipnir, Arsenal Birds等)を超兵器として認定している。
acecombat.wiki.gg は同じ対象を Disputed として独自判定しているが、
本プロジェクトでは公式判定を一次採用とする。

### それ以外のAC作品内の兵器情報(性能、設定、登場作品等)

1. **acecombat.wiki.gg** (英語AC Wiki、Project Aces監修)
2. **acecombat.fandom.com** (旧Fandom版。wiki.gg未掲載項目の補完用)
3. **acecombat.jp/en/chronicle/weapons/** (公式 ACES CHRONICLE)

### 作品別の登場機体クロスチェック

- **dic.nicovideo.jp/t/a/エースコンバットシリーズの登場機体**
- 用途: 各作品にどの機体が出るかを表でクロスチェック
- 注意: ユーザー編集ソースのため、機体の性能・設定詳細は1〜3で裏取りすること
- AC Infinity は 2018年3月でサービス終了済み。2021年時点の情報で確定とみなす
- AC8 (Wings of Theve) は本DBのスコープ外(未発売)

### 実在兵器

- 該当国の公式資料 > 信頼できる軍事資料(Jane's等) > 英語版Wikipedia

### 典拠として認めないもの

- 個人ブログ、Steam Workshop投稿者コメント、Reddit、ファン創作Wiki
- 上記をやむを得ず参照する場合は出典URLを明記の上、「信頼度: 低」と注記する

### 複数典拠で情報が食い違う場合

- 優先度の高いソースを採用する
- 食い違いの内容を備考に残す

---

## 4. 項目スキーマ(各エントリで必須)

各エントリは以下の形式で記述する:

```markdown
### アリコーン (Alicorn)

- **種別**: 架空艦 / 超兵器搭載艦
- **登場作品**: AC7 (DLC SPミッション)
- **製造主体(in-universe)**: ユークトバニア(建造) → エルジア(運用)
- **性質**: 全長495mの大型原子力潜水艦。潜水航空巡洋艦。
  主砲に大型レールキャノン1基、副砲に2基のレールガン砲塔。
  船体内に48基のVLS。船体上部に滑走路、艦橋部に16基のSLUAV発射ベイ。
- **典拠URL**:
  - https://acecombat.wiki.gg/wiki/Alicorn
- **HOI4実装判定**: △ アーキタイプ追加で可能
- **判定根拠**: 機体本体の追加方針外だが、搭載兵器(レールキャノン、
  レールガン、SLUAV)はモジュール化可能なため、艦本体は△、各兵器は
  別エントリで○とする
- **MD既存実装との対応**: 未確認
  (要 grep 確認: missile_submarine, attack_submarine の派生として実装可能か)
- **備考**: 設定上「量産されず1隻のみ」。HOI4で量産可能にするか、
  Special Projects経由の1点物にするかは要判断
```

### 各フィールドの記載ルール

- **名称**: 日本語(英語原語)。日本語表記が一般化していない場合は英語原語のみでも可
  (その場合「日本語訳なし」と備考に記す)
- **種別**: 以下のいずれか:
  - `超兵器` (公式 superweapon リストおよび CBRN リスト掲載のもの)
  - `架空機` (in-universe 架空の航空機)
  - `架空艦` (in-universe 架空の艦船)
  - `モジュール構成要素` (機体・艦の一部の特徴的システム。COFFIN、APS、TLS 等)
  - `実在機` (実在の航空機)
- **登場作品**: 略称使用可。AC2, AC3, AC04, AC5, ACZ, ACX, AC6, ACJA, AH, AC7, ACI
- **製造主体**: 設定上の製造国・組織。不明なら「不明」
- **性質**: 3〜5行で簡潔に。設定本文の丸写しは禁止
- **典拠URL**: 1つ以上必須。書ける情報すべてのソースを列挙
- **HOI4実装判定**: 以下のいずれか:
  - `○ モジュール化可`: 既存アーキタイプのスロットに新モジュールとして追加可能
  - `△ アーキタイプ追加`: 専用アーキタイプ追加が必要(機体・艦本体相当)。
    MD既存実装パターン(mothership_equipment + Special Projects)に倣う
  - `× 除外`: 固定設置型・通常兵装・実装方針外
  - `要判断`: 判定するための情報が不足、または方針との整合が要相談
- **判定根拠**: なぜその判定かを1〜2行で
- **MD既存実装との対応**: 該当する既存のMDコード(ファイル名・行番号)
  - 未調査なら「未確認」
  - grepしてヒットしなければ「該当なし(grep済み: キーワード)」
- **備考**: ハルシネーション注意点、判断が割れる可能性のある点、設定上の曖昧さなど

---

## 5. 作業順序

種別ごとに作業を進める。順番:

1. **超兵器**(公式 superweapon リストおよび CBRN リスト掲載のもの)
2. **架空機**(in-universe 架空の航空機)
3. **架空艦**(in-universe 架空の艦船。アリコーン、シンファクシ級など)
4. **モジュール構成要素**(機体・艦の一部の特徴的システム。COFFIN、APS、TLS等)
5. **実在機**(MD既存実装との突き合わせ)

各種別内では、AC作品の発表順:
**AC2 → AC3 → AC04 → AC5 → ACZ → ACX → AC6 → ACJA → AH → AC7 → ACI**

種別が一段落するごとに進捗報告し、次の種別に進む前に判断が必要な点を整理する。

---

## 6. 範囲(対象作品)

### DBに含める作品(11作品)

- AC2 (Ace Combat 2)
- AC3 Electrosphere
- AC04 (Shattered Skies)
- AC5 (The Unsung War)
- ACZ (Zero: The Belkan War)
- ACX (X: Skies of Deception)
- AC6 (Fires of Liberation)
- ACJA (Joint Assault)
- AH (Assault Horizon)
- AC7 (Skies Unknown)
- ACI (Infinity)(2018年3月サービス終了済み。情報は確定とみなす)

### スコープ外

- AC8 (Wings of Theve、未発売)
- AHL (Assault Horizon Legacy)
- AC Xi
- AC Northern Wings
- AC Advance

---

## 7. ハルシネーション防止ルール(厳守)

以下を全エントリで徹底する:

- 典拠URLが提示できないエントリは作らない。例外なし
- 「~らしい」「~と思われる」「~の可能性が高い」等の推測表現を本文で禁止
  判定根拠や備考で「典拠不明」と明記する形に統一
- 不明な項目は空欄にせず「未確認」「該当なし」「不明」のいずれかを明記
- AC作品の「通常兵装」(4AAM、6AAM、QAAM、SAAM、マルチロックオン系、
  SOD/SOG/UGB 等の通常特殊兵装)は対象外
- 固定設置型超兵器は判定× だが、エントリ自体は作成する(判断材料として残す)
- 架空車両・実在車両・実在艦のエントリは作らない(スコープ外)
- ハルシネーションを誘発しやすい項目(性能数値、製造年、運用国の細かい変遷など)は
  典拠URLからの引用元を明示し、典拠が示せないなら省略する
- 複数典拠で情報が食い違う場合は、優先度の高いソースを採用し、
  食い違い内容を備考に残す
- 超兵器分類は公式(acecombat.jp)を一次優先。
  acecombat.wiki.gg の Disputed 判定は採用しない
- AI の「記憶」に頼った記述を禁止。
  確実に typecheck していない情報は書かない

---

## 8. MD既存実装との対応の調べ方

MDリポジトリ内の実コードを当たり、対応物がある場合はファイル名+行番号を残す。

### 主要ファイル

- `common/units/equipment/MD_plane_airframes.txt` (航空機アーキタイプ)
- `common/units/equipment/MD_mtg_ships.txt` (艦船)
- `common/units/MD_naval_units.txt` (艦種定義)
- `common/units/MD_air_units.txt` (航空機種定義)
- `common/units/equipment/MD_tank_chassis.txt` (戦車シャシー)
- `common/units/equipment/MD_missiles.txt`
- `common/units/equipment/MD_guided_missiles.txt`
- `common/units/equipment/MD_ballistic_missiles.txt`
- `common/units/equipment/MD_sam_missile.txt`
- `common/units/equipment/MD_nuclear_missiles.txt`
- `common/units/equipment/modules/` (全モジュール)
- `common/special_projects/projects/` (Special Projects、`air_projects.txt` 等)

### 検索方法

- 兵器名・特徴的キーワードで `grep -rin "keyword" common/` を実行
- ヒットしたら、ファイルパス+行番号を記録
- ヒットしなかったら「該当なし(grep済み: キーワード)」と記録

### 既存実装の参考例(先行例として記録済み)

- アーセナルバード → `mothership_equipment`
  - 場所: `common/units/equipment/MD_plane_airframes.txt`, line 6086 付近
- アーセナルバード入手フロー → `sp_arsenal_bird`
  - 場所: `common/special_projects/projects/air_projects.txt`, line 651 付近

### 重要

MDコード上の対応は典拠URLではなく「ファイルパス+行番号」で残す。
両者は別欄に書き分けること。

---

## 9. 完成判定

DATABASE.md が以下を満たしたら完成とする:

- すべてのエントリに典拠URLが付いている
- すべてのエントリにHOI4実装判定(○ / △ / × / 要判断)が付いている
- MD既存実装との対応は以下のいずれかが明記されている:
  - 「対応物あり(パス+行番号)」
  - 「該当なし(grep済み: キーワード)」
  - 「未確認」
- 「未確認」「要判断」のエントリは末尾の「要追加調査リスト」に集約されている

最後に、Claude Code/Codex は以下の統計レポートを DATABASE.md 末尾に追記する:

- エントリ総数
- 種別別件数(超兵器、架空機、架空艦、モジュール構成要素、実在機)
- 判定別件数(○ モジュール化可、△ アーキタイプ追加、× 除外、要判断)
- MD対応状況(対応物あり、該当なし、未確認 の件数)
- 要追加調査リストの件数

---

## 10. 進捗報告

種別が完了するたびに以下を報告する:

- 完了種別(例: 「超兵器セクション完了」)
- そのセクションのエントリ数
- 判定別の内訳
- 不明点・判断が分かれそうな点(あれば)
- 次の種別に進んでよいかの確認

### 不明点・判断保留が出た場合

その時点で作業を止め、ユーザに判断を仰ぐ。勝手に判定しない。

### 特に判断を仰ぐべきケース

- 設定上「量産されなかった1点物」かつ「機体本体」のもの(△か×かでブレるケース)
- MD既存実装と機能が被るもの(棲み分けが必要)
- 典拠URLは見つかったが内容が公式と二次創作で食い違うもの
- 公式とacecombat.wiki.gg で超兵器分類が割れている場合
  (本プロジェクトでは公式優先だが、その都度判断を仰ぐ)
