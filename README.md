# Local Topology Manager 🌐

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

### ✨ Features

#### 1. 🗄️ Purpose-Built Bulk Editor (`editor.html`)
- A dedicated tabular UI optimized for network configuration, featuring specific actions like **cable Swapping (⇄)** and **row reordering (⬆️⬇️)**.
- **Port & IP Management:** Manage physical ports, MAC addresses, and IP addresses (including Virtual IPs and Loopbacks) for each device.
- **Smart Suggestion:** Automatically suggests available ports based on the selected device to prevent typos, along with device roles/locations.

#### 2. 🗺️ Interactive Topology Viewer (`viewer.html`)
- Powered by `Mermaid.js`. Generates beautiful physical topology maps instantly.
- **Hop-based Depth Control:** Limit the visualization depth by hop count, keeping large physical networks clean and readable.
- **Intuitive Canvas:** Smooth zoom & pan controls targeting the mouse cursor for easy navigation. Click any device to see connected ports and IP details.

#### 3. 🏢 Physical Riser Viewer (`viewer-riser.html`) [Beta]
- Generates a physical riser diagram (elevation drawing) to visualize inter-floor backbones and real-world cable routing.
- Solves the "spaghetti wiring" problem by implementing realistic "ceiling-bus" and "vertical-duct" routing algorithms, avoiding overlapping cables and intersecting boxes.

#### 4. 📓 Optimized Export for Notion
- Generates perfectly formatted CSVs for Notion databases.
- **Hybrid Display:** Connected ports are listed vertically with details, while available ports are grouped horizontally in a compact "tag" style, keeping your Notion tables clean and highly readable.

#### 💡 CSV Output Image (Inside a Notion cell)
```text
【Connected】
🔗 LAN1: [IP:192.168.0.1, MAC:00:11...] ↔ (WAN) Core Router
🔗 LAN2: ↔ (Uplink) Access Switch

【Available】
[LAN3] [LAN4] [LAN5] [LAN6] [LAN7] [LAN8]
```

### 🚀 Quick Start (Local)

You can try it out immediately without installing anything!

1. **Clone or Download** this repository.
2. Open `editor.html` in your web browser (Chrome/Edge/Safari).
3. Click **"📂 Load YAML File"** and select `sample_project.yaml`.
4. Edit the devices, ports, or cables, then click **"💾 Export YAML"** to save your changes.
5. Open either `viewer.html` or `viewer-riser.html` in your browser, load the saved YAML, and click **"✨ Render Topology"**.

### 🌍 Host as a Web App (GitHub Pages)

Since this tool consists purely of HTML and Vanilla JS, you can instantly host it for free using **GitHub Pages**.
1. Fork or push this repository to your GitHub account.
2. Go to `Settings` > `Pages`.
3. Select `main` branch as the source and save.
4. Your topology manager is now live on the web, with 100% data processing done safely and locally in the browser!

### 📂 Data Structure (YAML)

The master data consists of three simple arrays: `devices`, `cables`, and `locations`. Highly readable and IaC-friendly.

```yaml
devices:
  - id: dev_001
    name: ISP Modem 01
    role: ONU
    location_id: loc_f1
    tags: WAN
    ports:
      - name: LAN1
        ip: 192.168.0.1
        mac: "00:11:22:AA:BB:01"

cables:
  - id: cab_001
    device_a_id: dev_001
    port_a: LAN1
    device_b_id: dev_002
    port_b: WAN

locations:
  - id: loc_f1
    name: Floor 1
    parent_id: null
```

## 日本語ドキュメント

### 💡 開発の背景
通常、物理ネットワーク構成（機器、結線、設置場所）を管理するにはNetBoxのような重厚なサーバーを立てるか、SaaSを契約する必要がありますが、小規模〜中規模のプロジェクトにおいてはそれ自体が技術的負債になりがちです。

本ツールは **「1つのYAMLファイルをマスターデータとして管理する」** アプローチを採用しています。環境構築は一切不要で、HTMLをブラウザで開くだけで実運用に耐えうる構成・IPアドレス管理が実現できます。必要なライブラリを同梱しているため、**完全なオフライン環境（閉域網・エアギャップ）** にも対応。電波の届かないデータセンターの地下やセキュリティの厳しい現場でも、USBメモリで持ち込んで即座に稼働します。

### ✨ 特徴

#### 1. 🗄️ ネットワーク管理に特化したEditor (`editor.html`)
- 専用のテーブルUIで大量のデータを効率よく編集できます。**結線の左右入れ替え（SWAP: ⇄）** や **行の上下移動（⬆️⬇️）** など、構成管理に便利なアクションを備えています。
- **ポート・IP管理:** 機器ごとにポート、IPアドレス（仮想IP・ループバック含む）、MACアドレスを管理できる専用UIを搭載。
- **サジェスト機能:** 結線入力時、選択した機器が持つポートだけを賢くサジェストし、入力ミスを防ぎます。

#### 2. 🗺️ インタラクティブなViewer (`viewer.html`)
- `Mermaid.js` による自動描画で、美しい物理トポロジー図を瞬時に生成します。機器をクリックするとポートやIPの詳細情報も確認できます。
- **ホップ数による深さ制限:** 起点から何台先まで描画するか（ホップ数）を制御可能。規模の大きなネットワーク構成でも見やすく保つことができます。
- マウスカーソルを中心に拡大・縮小できる、直感的で滑らかなキャンバス操作を実装しています。

#### 3. 🏢 物理・立面系統図ビューア (`viewer-riser.html`) [Beta]
- 実際の建築設備における「縦幹線（ライザー）と各階への配線ルート」を直感的に可視化する物理系統ビューアです。
- 現実のケーブルラックやダクト配線を模倣した独自アルゴリズムにより、線が機器を貫通するカオス状態（スパゲッティ配線）を自動で回避し、図面としての美しさと実用性を両立しています。

#### 4. 📓 Notionへのシームレスな出力（ハイブリッド表示）
- Notion等の資産管理データベースへインポートするためのCSVを出力します。
- **ハイブリッド表示:** 接続済みポートは詳細に、空きポートは横並びの「タグ形式」で出力することで、Notion上での視認性と管理性を美しく両立しています。

#### 💡 CSV出力のイメージ（Notionの1セル内）
```text
【Connected】
🔗 LAN1: [IP:192.168.0.1, MAC:00:11...] ↔ (WAN) Core Router
🔗 LAN2: ↔ (Uplink) Access Switch

【Available】
[LAN3] [LAN4] [LAN5] [LAN6] [LAN7] [LAN8]
```

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

### 📂 データ構造 (YAML)

マスターデータは `devices`、`cables`、`locations` の3つのシンプルな配列で構成されています。人間が直接読み書きしやすいフォーマットです。

```yaml
devices:
  - id: dev_001
    name: ISP Modem 01
    role: ONU
    location_id: loc_f1
    tags: WAN
    ports:
      - name: LAN1
        ip: 192.168.0.1
        mac: "00:11:22:AA:BB:01"

cables:
  - id: cab_001
    device_a_id: dev_001
    port_a: LAN1
    device_b_id: dev_002
    port_b: WAN

locations:
  - id: loc_f1
    name: Floor 1
    parent_id: null
```
