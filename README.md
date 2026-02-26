# Local Topology Manager (v2.0) 🌐

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Air-Gap Ready](https://img.shields.io/badge/Air--Gap-Ready-success)
![Zero Dependency](https://img.shields.io/badge/Dependencies-Zero-success)
![Vanilla JS](https://img.shields.io/badge/Tech-Vanilla_JS-yellow)

**🚀 Live Demo: [Try the Editor](https://onishigyo.github.io/local-topology-manager/editor.html) | [Try the Viewer](https://onishigyo.github.io/local-topology-manager/viewer.html) | [Try the Riser Viewer (Beta)](https://onishigyo.github.io/local-topology-manager/viewer-riser.html)**

A Zero-dependency, Air-gap Ready, Local-first Physical Network Topology Editor & Viewer.

(日本語のドキュメントは下部にあります 👇)

## English Documentation

### 💡 Concept
Maintaining physical network topologies (Devices, Cables, Locations) usually requires heavy infrastructure like NetBox or proprietary SaaS. However, for small to medium projects, managing servers or databases often becomes a technical debt.

**"Data as Code" approach is the answer.** This tool allows you to manage your entire physical infrastructure via a single YAML file. It runs entirely in your local browser with **Zero Dependencies**. Since core libraries are bundled, it is **100% Air-gap Ready**—perfect for secure datacenters or MDF rooms with no internet access.

### ⚠️ Important Concepts & Rules (v2.0)
To keep the topology clean and prevent data chaos, please follow these 3 operational rules:

#### 1. System Categorization Rule
Assign strictly ONE `System` to each device (multi-tags have been abolished).
* **End Devices (Dedicated):** Assign specific system names like `Lighting`, `HVAC`, or `BMS`.
* **Shared Infrastructure:** Do NOT assign specific system names to shared pipes (e.g., ONUs, Core Routers, Floor L2SWs). Leave them blank or set them to `Infra`.
* **Security & Maintenance:** Devices monitoring the entire network should be set to `Security` or `Maintenance`.

#### 2. Smart Trace & Multi-Path Routing
When you filter by a specific system (e.g., `Lighting`) in the Viewer, the "Smart Trace" kicks in:
1. **Target highlighting:** Target devices (`Lighting`) are highlighted in **blue**.
2. **Multi-Path Tracing:** The tool automatically traces ALL upstream physical paths to the core network (ONU). It seamlessly handles redundant links and shared security appliances (e.g., a firewall spanning multiple VLANs/networks).
3. **Bystander styling:** Any intermediate shared infrastructure or devices belonging to other systems that act as a "pass-through" are automatically drawn in **white** standard boxes.
> 💡 **Meaning:** You don't need to manually tag intermediate switches! The UI keeps your focus entirely on the selected system.

#### 3. 3-Level Location Input
In the Editor, locations should be managed using the following 3-tier hierarchy:
* **Level 1 (Floor):** e.g., `1F`, `2F`
* **Level 2 (Room):** e.g., `MDF Room`, `Fan Room`
* **Level 3 (Detail):** e.g., `Rack A`, `Control Panel 2`

### ✨ Features

#### 1. 🗄️ Purpose-Built Bulk Editor (`editor.html`)
- A dedicated tabular UI optimized for network configuration, featuring specific actions like **cable Swapping (⇄)** and **row reordering (⬆️⬇️)**.
- **Strict Role Management:** Uses a standardized 11-role dropdown (e.g., `Router`, `Core Switch`, `Switch / Hub`) to maintain data integrity.
- **Port & IP Management:** Manage physical ports, MAC addresses, and IP addresses (including Virtual IPs and Loopbacks) for each device.

#### 2. 🗺️ Interactive Topology Viewer (`viewer.html`)
- Powered by `Mermaid.js`. Generates beautiful physical topology maps instantly.
- **Hop-based Depth Control:** Limit the visualization depth by hop count, keeping large physical networks clean and readable.

#### 3. 🏢 Physical Riser Viewer (`viewer-riser.html`) [Beta]
- Generates a physical riser diagram (elevation drawing) to visualize inter-floor backbones and real-world cable routing.
- Solves the "spaghetti wiring" problem by implementing realistic "ceiling-bus" and "vertical-duct" routing algorithms, avoiding overlapping cables and intersecting boxes.

#### 4. 📓 Optimized Export for Notion
* Generates perfectly formatted CSVs for Notion databases.
* Features a **Hybrid Display**: connected ports are listed vertically with details, while available ports are grouped as compact tags.
<details>
<summary>👀 Click to see CSV Output Example</summary>

```text
【Connected】
🔗 LAN1: [IP:192.168.0.1, MAC:00:11...] ↔ (WAN) Core Router
🔗 LAN2: ↔ (Uplink) Access Switch

【Available】
[LAN3] [LAN4] [LAN5] [LAN6] [LAN7] [LAN8]
```
</details>

---

## 日本語ドキュメント

### 💡 開発の背景
通常、物理ネットワーク構成（機器、結線、設置場所）を管理するにはNetBoxのような重厚なサーバーを立てるか、SaaSを契約する必要がありますが、小規模〜中規模のプロジェクトにおいてはそれ自体が技術的負債になりがちです。

本ツールは **「1つのYAMLファイルをマスターデータとして管理する」** アプローチを採用しています。環境構築は一切不要で、HTMLをブラウザで開くだけで実運用に耐えうる構成・IPアドレス管理が実現できます。必要なライブラリを同梱しているため、**完全なオフライン環境（閉域網・エアギャップ）** にも対応。電波の届かないデータセンターの地下やセキュリティの厳しい現場でも、USBメモリで持ち込んで即座に稼働します。

### ⚠️ 重要な概念と運用ルール (v2.0 新仕様)
構成図を破綻させずに綺麗に管理するため、以下の3つの運用ルールを設けています。

#### 1. 設備システム (System) の登録ルール
各デバイスには「どのシステムに属しているか」を示す **System** を1つだけ設定します（複数タグの廃止）。
* **末端機器（専用機器）**: `Lighting`, `HVAC`, `BMS` などのシステム名を設定します。
* **共有インフラ**: 複数のシステムが相乗りする土管（ONU、コアルーター、各階L2SWなど）には、特定のシステム名を設定しないでください。代わりに `Infra` と入力するか、未設定にします。
* **監視・保守機器**: インフラ全体を監視する機器は `Security` や `Maintenance` と設定します。

#### 2. スマート・トレース機能（複数経路の自動逆算）
Viewerで特定のシステム（例：`Lighting`）を選択して描画すると、以下の「スマート・トレース」が発動します。
1. `Lighting` に設定された主役機器が**青色**でハイライトされます。
2. その機器から、大元（ONU等）へ向かう物理ケーブルの経路をプログラムが「すべて」自動的に逆算します。**冗長化された経路や、複数のネットワークをまたいで監視するセキュリティ機器などの複雑な相乗り構成も正確に分岐して描画します。**
3. 経路上にある共有インフラや、他システムの機器が「ただの通り道」として使われる場合、それらは自動的に巻き込まれ、目立たない**白色**の通常枠として描画されます。
> 💡 **つまり「主役は青、脇役は白」の究極にシンプルなUIにより、人間がわざわざ途中のルーターやスイッチの設定を気にする必要はありません！**

#### 3. ロケーション (Location) の3階層入力
デバイスの配置場所は、Editor上で以下の3階層に分けて入力・管理します。
* **Level 1 (フロア)**: `1F`, `2F`, `4F` など
* **Level 2 (部屋)**: `ファンルーム`, `MDF室`, `事務室` など
* **Level 3 (詳細)**: `システム制御盤2`, `ラックA` など

### ✨ 特徴

#### 1. 🗄️ ネットワーク管理に特化したEditor (`editor.html`)
- 専用のテーブルUIで大量のデータを効率よく編集できます。**結線の左右入れ替え（SWAP: ⇄）** や **行の上下移動（⬆️⬇️）** など、構成管理に便利なアクションを備えています。
- **厳格なRole管理:** 表記揺れを防ぐため、11種類の標準的な役割（`Router`, `Core Switch`, `Switch / Hub`など）から選択するプルダウン方式を採用しています。
- **ポート・IP管理:** 機器ごとにポート、IPアドレス（仮想IP・ループバック含む）、MACアドレスを管理できる専用UIを搭載。
- **サジェスト機能:** 結線入力時、選択した機器が持つポートだけを賢くサジェストし、入力ミスを防ぎます。

#### 2. 🗺️ インタラクティブなViewer (`viewer.html`)
- `Mermaid.js` による自動描画で、美しい物理トポロジー図を瞬時に生成します。機器をクリックするとポートやIPの詳細情報も確認できます。
- **ホップ数による深さ制限:** 起点から何台先まで描画するか（ホップ数）を制御可能。規模の大きなネットワーク構成でも見やすく保つことができます。

#### 3. 🏢 物理・立面系統図ビューア (`viewer-riser.html`) [Beta]
- 実際の建築設備における「縦幹線（ライザー）と各階への配線ルート」を直感的に可視化する物理系統ビューアです。
- 現実のケーブルラックやダクト配線を模倣した独自アルゴリズムにより、線が機器を貫通するカオス状態（スパゲッティ配線）を自動で回避し、図面としての美しさと実用性を両立しています。

#### 4. 📓 Notionへのシームレスな出力（ハイブリッド表示）
* Notion等の資産管理データベースへインポートするためのCSVを出力します。
* **ハイブリッド表示**を採用：接続済みポートは詳細に、空きポートは横並びの「タグ形式」で出力し、視認性と管理性を両立します。
<details>
<summary>👀 CSV出力イメージを見る</summary>

```text
【Connected】
🔗 LAN1: [IP:192.168.0.1, MAC:00:11...] ↔ (WAN) Core Router
🔗 LAN2: ↔ (Uplink) Access Switch

【Available】
[LAN3] [LAN4] [LAN5] [LAN6] [LAN7] [LAN8]
```
</details>

### 🚀 使い方 (ローカル環境)

環境構築は不要です。ダウンロードして数秒で試せます。

1. このリポジトリを **Clone または Download** します。
2. ブラウザで `editor.html` を開きます。
3. **「📂 Load YAML File」** をクリックし、同梱の `sample_project.yaml` を読み込みます。
4. 機器や配線、ポート情報を自由に追加・編集し、**「💾 Export YAML」** で手元に保存します。
5. ブラウザで `viewer.html` または `viewer-riser.html` を開き、保存したYAMLを読み込んで **「✨ トポロジーを描画する」** や **「✨ 描画更新」** をクリックします。

### 🌍 チーム向けにWebアプリとして公開する (GitHub Pages)

本ツールはHTMLとVanilla JSのみで構成されているため、**GitHub Pages** を使えば無料で瞬時にWebアプリとしてチーム内に公開できます。
1. このリポジトリを自身のGitHubアカウントにFork（またはPush）します。
2. リポジトリの `Settings` > `Pages` を開きます。
3. Sourceとして `main` ブランチを選択して保存します。
4. 数分後にはURLが発行され、どこからでもアクセス可能な構成管理ツールになります！（※データ処理はすべてブラウザのローカルで行われるため、機密データが外部サーバーに送信されることはなく安全です）
