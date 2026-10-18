# PS5-P2PINFO

[English](README.md) | [한국어](README.ko.md) | **日本語**

PS5-P2PINFOは、ミラーリングされたPS5の有線LANトラフィックを利用して、Armored CoreのP2P接続状態を監視するWindowsアプリケーションです。

## デモ動画

対戦開始前の紹介画面をOCRで読み取り、相手のパイロット名とPSN IDを表示しながら接続状態を監視する様子です。再生ボタンを押してご覧ください。

https://github.com/user-attachments/assets/407d5249-2e13-4ceb-be3f-f961211cb531

## ダウンロード

- [最新リリースページ](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest)
- [PS5-P2PINFO-Setup-x64.exeを直接ダウンロード](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest/download/PS5-P2PINFO-Setup-x64.exe) — 一般的なIntel・AMD PC向け
- [PS5-P2PINFO-Setup-arm64.exeを直接ダウンロード](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest/download/PS5-P2PINFO-Setup-arm64.exe) — ARM64実機で未テストの実験的ビルド

この `.exe` はポータブル実行ファイルではなく、Inno Setupで作成されたインストーラーです。アプリケーションのインストール、選択したショートカットの作成、Windowsの標準アンインストール項目の登録を行います。.NETを含むself-contained形式のため、.NETを別途インストールする必要はありません。

現在、インストーラーにはデジタルコード署名がないため、Windows SmartScreenの警告が表示される場合があります。上記のGitHub公式リリースからのみダウンロードしてください。

## 主な機能

- パイロット名・PSN IDとPing、RTT Jitter、試合全体の累積Packet Lossを表示。名前が不明でも`-`と表示してネットワーク測定を続けます。
- オプションのOBS連携と英語・韓国語・日本語のローカルPaddleOCR
- 試合前の紹介画面を自動認識。結果画面は **OCR Selected Match** ボタンで認識
- 開始時刻・本人確認情報・終了状態・接続推定値を保存するHistoryと **Edit Identity...** による修正
- 相手のIP・範囲・ポート表示、IP Map、IP検索、IPによる相手の推定はありません。履歴にもネットワークアドレスを保存しません。

`RTT Jitter`は直近60秒内の隣接する生RTT値の絶対差の平均です。最低6サンプル（5変化）が必要なため、最初は`Collecting`と表示され、通常約50秒かかることがあります。古い完成値は`stale`表示になります。Packet Lossは読み取れるゲームパケットの連番欠落を使い、読み取れない場合はprobe応答のタイムアウトを使って試合全体で累積します。

> [!NOTE]
> ミラーリングされたPS5トラフィックから計算する参考推定値であり、本体の直接測定値ではありません。RTTには往復経路と相手側の応答処理が含まれます。ミラーリング・アダプター・PCの取りこぼしも損失推定に影響し、probe間の短い途切れは検出できない場合があります。

## OBS連携とOCRの設定（任意）

OBSがなくてもネットワーク監視は利用できます。OCRには、HDMI/USBキャプチャデバイスなどを通じて、PS5のゲーム映像が **同じWindows PC・ユーザーアカウントのOBS** に入力されている必要があります。

1. [OBS Studio](https://obsproject.com/download) 28以降をインストールします。**ソース → + → 映像キャプチャデバイス**で映像を追加するか、既存のゲーム映像ソースを使います。ゲーム画面全体が隠れず、切れずに表示されることを確認します。
2. OBSの **ツール → WebSocketサーバー設定** でサーバーを有効にし、認証は有効のまま設定を適用します。OBS 28以降はWebSocketを内蔵しています。[OBS公式ガイド](https://obsproject.com/kb/remote-control-guide)も参照してください。
3. PS5-P2PINFOの **Config → OBS & OCR → My PSN ID** に自分のオンラインIDを入力し、**Save Config** を押します。
4. **Connect OBS** を押して、一覧からゲーム映像ソースを自分で選択します。特定言語のソース名やデバイス名を前提にしません。選択は保存され、次の接続時に同じソースがあれば再利用します。
5. **Automatically read pre-match introductions** を有効にし、次の試合前に **Start Capture** を押します。新しい試合から最大90秒間、紹介カードをメモリ内で確認し、両者のカードが安定して読み取れ、自分のIDが一意に一致すると停止します。**カスタムマッチなどでパケットからパイロット名を取得できた試合では自動OCRを行いません。名前のパケットが後から届いた場合も自動認識を中止します。**
6. 結果画面をOBSに表示したまま **Historyの該当試合を選択 → OCR Selected Match** を押します。結果画面は自動認識しません。読み取り中に別の行を選んでも、最初に選んだ試合へ保存します。カスタムマッチでも手動OCRを使用できます。
7. 誤認識がある場合は結果画面を表示して **Config → Calibrate Regions...** を開きます。パイロット・機体名の言語をEnglish/Korean/Japaneseから選び、6つの領域を調整して **Save Regions** を押します。PSN IDは常に英語モデルを使います。Historyの **Edit Identity...** でも修正できます。

ソースは縦横比を維持した1920×1080で取得します。プロジェクターウィンドウ、配信、録画は不要です。同梱モデルを使ってPC内で処理し、スクリーンショットは保存しません。自動認識は対応するシングルマッチの紹介レイアウトが対象です。チーム戦・別レイアウト・紹介を見逃した場合は結果OCRを使用してください。複数の試合が同時に検出された場合、その試合の自動関連付けを停止します。

### OBS接続のトラブルシューティング

- OBSとWebSocketサーバーを起動した状態にしてください。ソースや設定を変更したら **Reconnect OBS** を押します。
- 通常は `%APPDATA%/obs-studio/plugin_config/obs-websocket/config.json` からローカルポートとパスワードを読みます。アプリ設定やログにパスワードをコピーしません。他人に渡す必要もありません。
- ポータブル版や特殊な配置では **OBS settings file → Browse...** で該当するWebSocketの`config.json`を指定し、保存して再接続します。通常はポータブルOBSフォルダー内の`config/obs-studio/plugin_config/obs-websocket/config.json`です。通常のインストールでは空欄にします。
- 別PCのリモートOBSには対応しません。ソースの削除・名前変更後は再選択してください。黒い画面や切れた映像は先にOBS側で修正します。
- 自分のIDが見つからない、または2つある場合は相手を確定しません。**My PSN ID**、画質、6つの領域を確認してください。左右の位置だけで自分を判断しません。

### 履歴とローカルファイル

設定、`ocr-settings.json`、`history.json`は `%LOCALAPPDATA%/PS5-P2PINFO` に保存されます。認識可能な試合トラフィックで行を作り、通信停止後またはキャプチャ停止時に同じ行を完了します。異常終了した行は次回起動時に`Interrupted`になります。確定した情報は終了処理でも維持し、試合ごとの記録とパイロット名の別名は区別します。IPから次の相手を推定しません。**Open History Folder**で保存先を開けます。実行ファイルの隣の`OcrData`フォルダーを保持してください。Pythonや別途OCRのダウンロードは不要です。

![架空データによるHistory](assets/history.png)
![個人設定を含まないOBS設定画面](assets/obs-config.png)

## 関連ポリシーおよび利用規約

適用されるポリシーおよび利用規約は、国または地域によって異なります。この日本語版では、日本に居住するユーザーおよび日本のPlayStationアカウントに適用される公式リンクを案内しています。ほかの国または地域で利用する場合は、その地域の最新規約をご確認ください。ゲームまたはサービス内で個別に提示される規約も適用される場合があります。

PS5-P2PINFOは、適用される法律、プラットフォームポリシーおよびサービス利用規約を尊重し、遵守することを目的として設計されています。

- [PlayStation Network利用規約 – 日本](https://www.playstation.com/ja-jp/legal/psn-terms-of-service/)
- [PlayStationソフトウェアアプリケーション使用許諾契約 – 日本](https://www.playstation.com/ja-jp/legal/software-eula/)
- [PS5用システムソフトウェア使用許諾契約 – 日本](https://www.playstation.com/ja-jp/legal/ps5-ssla/)

## 対応環境

- [.NETの公式サポート対象OS](https://github.com/dotnet/core/blob/main/release-notes/10.0/supported-os.md)に現在含まれるWindows 11 x64リリース — Windows 11 Homeビルド26200の実機で動作確認済み
- Windows 10 x64 — [.NETの公式サポート対象OS](https://github.com/dotnet/core/blob/main/release-notes/10.0/supported-os.md)に現在含まれるEnterpriseまたはLTSCリリースのみ。本プロジェクトではWindows 10実機で未テスト
- ポートミラーリングに対応したルーターまたはマネージドスイッチ
- PS5の送受信トラフィックが双方向とも監視PCへミラーリングされている環境
- **WinPcap API-compatible Mode**を有効にしてインストールした[Npcap](https://npcap.com/#download)
- インストーラーの実行に必要な管理者権限

> [!WARNING]
> Windows ARM64向けインストーラーは実験的ビルドとして個別に提供します。ビルドには成功していますが、ARM64実機ではテストしていません。そのため、ARM64でのインストール、Npcap連携、パケットキャプチャの互換性は未確認です。

Npcapを `Restrict Npcap driver's access to Administrators only` でインストールした場合、キャプチャ開始時にUACの承認が必要になることがあります。権限エラーが解消しない場合に限り、PS5-P2PINFOを管理者として実行してください。

## インストールと設定

このガイドでは、幅広い環境で利用しやすい「既存のWi-Fiルーター + TP-Link TL-SG108E」構成を使用します。TL-SG108EはWi-Fiルーターを置き換える機器ではありません。現在使用しているルーターのLANポートに追加で接続するため、スマートフォンやノートPCはこれまでどおり既存のWi-Fiを利用できます。

> [!IMPORTANT]
> 製品名末尾の `E` を必ず確認してください。`TL-SG108E` はポートミラーリングに対応していますが、`TL-SG108` はポートミラーリングに対応していない通常のアンマネージスイッチです。

### 1. 必要な機器を準備する

- 対応するx64 PC：現在サポート中のWindows 11、または.NETがサポートしているWindows 10 Enterprise/LTSCリリース
- 有線LANで接続するPS5
- 現在使用しているWi-Fiルーター
- [TP-Link TL-SG108E](https://www.tp-link.com/jp/business-networking/easy-smart-switch/tl-sg108e/) 8ポート ギガビット イージースマートスイッチ
- Cat5e以上のLANケーブル3本
- キャプチャ専用の **USBギガビットEthernetアダプター1台** — PCに有線LANポートがない場合は必要です。すでに有線ポートがある場合でも、キャプチャ中にPCの既存の有線インターネット接続を維持したいときは、**追加（予備）のアダプター**として使用すると便利です。PCの通常のインターネット接続にWi-Fiを使用する場合は、キャプチャ用Ethernetアダプター1台だけで十分です。

TL-SG108Eは、日本・韓国・米国の公式製品ページでポートミラーリング対応を確認でき、同じ公式設定ユーティリティを利用できます。日本公式ページの案内どおり、公開されているマニュアルと設定画面は英語です。このガイドでは、画面上の英語項目名をそのまま記載します。

| 地域 | 公式製品・サポート | 2026-08-10確認時の価格例 |
| --- | --- | --- |
| 日本 | [製品](https://www.tp-link.com/jp/business-networking/easy-smart-switch/tl-sg108e/)・[サポート／ダウンロード](https://www.tp-link.com/jp/support/download/tl-sg108e/) | [価格.com 約¥3,960から](https://kakaku.com/item/K0000886515/) |
| 韓国 | [製品](https://www.tp-link.com/kr/business-networking/easy-smart-switch/tl-sg108e/)・[サポート／ダウンロード](https://www.tp-link.com/kr/support/download/tl-sg108e/) | [Danawa 約36,000～43,000ウォン](https://prod.danawa.com/info/?pcode=4922979) |
| 米国 | [製品](https://www.tp-link.com/us/business-networking/easy-smart-switch/tl-sg108e/)・[サポート／ダウンロード](https://www.tp-link.com/us/support/download/tl-sg108e/) | [Lenovo 約US$29.99](https://www.lenovo.com/buy/us/en/easy-setup-switches-0avz00a) |

価格、在庫、送料は販売店や時期によって変わります。外観が似ている製品を購入する場合は、型番が正確に `TL-SG108E` であることを確認してください。

「ポートミラーリング」と「ポートフォワーディング」は別の機能です。製品説明にポートフォワーディングしか記載されていないルーターや、通常のアンマネージスイッチでは、この構成を作ることはできません。

### 2. PS5-P2PINFOインストーラーをダウンロードする

1. [最新のGitHubリリース](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest)を開きます。
2. **設定 → システム → バージョン情報 → システムの種類**を確認し、**Assets**から該当するインストーラーを選択します。
   - **x64ベース プロセッサ**: [`PS5-P2PINFO-Setup-x64.exe`](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest/download/PS5-P2PINFO-Setup-x64.exe) — 一般的なIntel・AMD PCはこちらを使用します。
   - **ARMベース プロセッサ**: [`PS5-P2PINFO-Setup-arm64.exe`](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest/download/PS5-P2PINFO-Setup-arm64.exe) — ARM64実機で未テストの実験的ビルドです。

<!-- Screenshot: assets/setup/common/01-github-release-assets.png
     Capture the release Assets list with both architecture-specific installers visible. -->

### 3. Npcapをインストールする

PS5-P2PINFOが有線パケットを受信するにはNpcapが必要です。Wiresharkは必要ありません。NpcapはPS5-P2PINFOのインストーラーに含まれていないため、ユーザー自身が公式サイトから直接インストールしてください。Npcap Free Editionは最大5台のシステムで利用でき、外部への再配布は許可されていません。この範囲を超えて利用する場合は、[Npcapの公式ライセンス情報](https://npcap.com/#download)をご確認ください。

1. [Npcap公式ダウンロードページ](https://npcap.com/#download)を開きます。
2. **Downloading and Installing Npcap Free Edition**から最新のNpcap installerをダウンロードします。
3. インストーラーを実行し、Windowsのユーザーアカウント制御が表示されたら **はい** を選択します。
4. **Install Npcap in WinPcap API-compatible Mode**が選択されていることを確認します。現在のNpcapでは既定で選択されていますが、必ず画面で確認してください。
5. `Restrict Npcap driver's access to Administrators only` は任意です。選択しない場合は標準ユーザーでも手軽にキャプチャでき、選択した場合はキャプチャ権限が管理者に制限され、開始時にUACの承認が必要になることがあります。
6. `Support raw 802.11 traffic` は、この有線ポートミラーリング構成では必要ありません。
7. インストールを完了し、再起動を求められた場合はWindowsを再起動します。

<!-- Screenshot: assets/setup/common/02-npcap-download.png
     Capture the current Free Edition installer link on the official Npcap page. -->
<!-- Screenshot: assets/setup/common/03-npcap-winpcap-mode.png
     Capture the Npcap options page with WinPcap API-compatible Mode selected. -->

各オプションの公式説明は、[Npcap Users' Guide](https://npcap.com/guide/npcap-users-guide.html#npcap-installation)で確認できます。

### 4. PS5-P2PINFOをインストールする

1. 上で選択したx64またはARM64のインストーラーを実行します。
2. Windows SmartScreenが表示された場合は、GitHubの公式リリースからダウンロードしたことを確認します。**詳細情報** を選択し、選択したファイル名が `PS5-P2PINFO-Setup-x64.exe` または `PS5-P2PINFO-Setup-arm64.exe` であることを再確認してから **実行** を選択します。コード署名がないため、発行元は **不明な発行元** と表示される場合があります。
3. Windowsのユーザーアカウント制御が表示されたら **はい** を選択します。
4. Npcapが必要であることを知らせる画面を確認し、**OK**を選択します。
5. **Select Destination Location**でインストール先を選択します。よく分からない場合は、既定の場所をそのまま使用します。
6. **Select Additional Tasks**でデスクトップショートカットが必要な場合は、**Create a desktop shortcut**を選択します。
7. **Ready to Install**で設定内容を確認し、**Install**を選択します。
8. インストールが完了したら、**Launch PS5-P2PINFO**を選択した状態で **Finish**を押します。

<!-- Screenshot: assets/setup/common/04-smartscreen.png -->
<!-- Screenshot: assets/setup/common/05-setup-npcap-notice.png -->
<!-- Screenshot: assets/setup/common/06-setup-destination.png -->
<!-- Screenshot: assets/setup/common/07-setup-additional-tasks.png -->
<!-- Screenshot: assets/setup/common/08-setup-ready.png -->
<!-- Screenshot: assets/setup/common/09-setup-complete.png -->

### 5. LANケーブルを接続する

以降の説明では、次のポート番号を使用します。同じ番号で接続すると、設定画面の手順をそのまま進められます。

```mermaid
flowchart TB
    Internet["インターネット"] --> Router["既存のWi-Fiルーター<br/>既存のWi-Fiはそのまま使用<br/>空きLANポートを使用（WAN以外）"]
    Router == LANケーブル1 - スイッチのポート1 ==> Switch["TP-Link TL-SG108E<br/>物理スイッチ1台<br/><br/>ポート1 = ルーター<br/>ポート2 = PS5<br/>ポート8 = キャプチャPC"]
    PS5["PS5<br/>有線LAN必須<br/>Wi-Fi接続ではありません"] == LANケーブル2 - スイッチのポート2 ==> Switch
    Switch == スイッチのポート8 - LANケーブル3 ==> PC["Windows監視PC<br/>キャプチャ用Ethernetアダプター"]

    classDef router fill:#f0fdf4,stroke:#16a34a,stroke-width:2px
    classDef switch fill:#eff6ff,stroke:#2563eb,stroke-width:3px
    classDef endpoint fill:#fff7ed,stroke:#ea580c,stroke-width:2px
    class Router router
    class Switch switch
    class PS5,PC endpoint
```

> [!NOTE]
> 図の太い3本の線だけが実際のLANケーブルです。`ポート2 → ポート8` のトラフィック複製は追加のケーブルではなく、TL-SG108Eの管理画面で行う設定です。手順7で説明します。

1. 既存のWi-Fiルーター背面にある空き **LAN** ポートと、TL-SG108Eの **ポート1** を接続します。ルーターの `WAN` または `Internet` ポートには接続しないでください。
2. PS5背面のLANポートと、TL-SG108Eの **ポート2** を接続します。
3. 監視PCのEthernetポートと、TL-SG108Eの **ポート8** を接続します。
4. TL-SG108Eの電源アダプターを接続し、約2分待ちます。
5. ケーブルを接続したポート1、2、8のリンクLEDが点灯または点滅していることを確認します。
6. PS5で **設定 → ネットワーク → 設定 → インターネット接続を設定 → 有線LANを設定 → 接続** の順に進み、有線接続を使用します。

[TP-Link日本公式製品ページ](https://www.tp-link.com/jp/business-networking/easy-smart-switch/tl-sg108e/)にも、ルーターとスイッチを接続する構成例が掲載されています。PS5をWi-Fiで接続するとポート2をトラフィックが通過しないため、この構成ではキャプチャできません。

> [!WARNING]
> ルーターとスイッチを2本のLANケーブルで重複接続しないでください。ネットワークループが発生する可能性があります。

### 6. TL-SG108Eの管理画面を開く

TL-SG108Eの画面は、ハードウェアバージョンやファームウェアによって多少異なる場合があります。最初に製品底面のラベルで、`Ver: 6.0` や `Ver: 6.6` などのハードウェアバージョンを確認してください。

1. [TL-SG108E日本公式サポートページ](https://www.tp-link.com/jp/support/download/tl-sg108e/)で、底面ラベルと一致するハードウェアバージョンを選択します。
2. 最新の **Easy Smart Configuration Utility**をダウンロードします。ZIP形式の場合は先に展開し、中にあるインストール用 `.exe` を実行します。
3. PCがTL-SG108Eのポート8に接続されている状態で、ユーティリティを起動します。
4. ユーティリティがネットワーク上のスイッチを自動検出するまで待ちます。表示されない場合は **Refresh**を選択します。
5. 検出された `TL-SG108E` の行にある **Login**アイコンを選択します。
6. 初回使用時にユーザー名とパスワードの作成を求められた場合は、画面の案内に従って新しい値を設定します。旧ファームウェアで既存のログイン情報を求められる場合、初期ユーザー名とパスワードがどちらも `admin` のことがあります。ログイン後、すぐにパスワードを変更してください。

<!-- Screenshot: assets/setup/common/10-tplink-discovery.png
     Capture TL-SG108E in Discovered Switches and point to Login. Mask the switch MAC address. -->

公式の[Easy Smart Configuration Utility User Guide - Discovering Switches](https://static.tp-link.com/upload/manual/2025/202510/20251023/1910013735_Easy%20Smart%20Configuration%20Utility_UG_1022.pdf#page=13)では、機器の検出、IPアドレスの確認、ログイン画面をスクリーンショット付きで確認できます。日本公式サポートページが案内するとおり、このUser Guideと設定画面は英語です。

### 7. PS5ポートの送受信トラフィックをミラーリングする

管理画面で **Monitoring → Port Mirror**を開き、次の値を設定します。

| 画面の項目 | 設定値 | 意味 |
| --- | --- | --- |
| `Port Mirror Status` | `Enable` | ポートミラーリングを有効にする |
| `Mirroring Port` または `Destination` | `8` | 複製されたトラフィックを受け取るPCのポート |
| `Mirrored Port` または `Source` | `2` | 監視対象となるPS5のポート |
| `Mirrored Mode` | `Both` | PS5の送信・受信を両方とも複製する |

ファームウェアの画面に `Both` がなく、ポートごとに `Ingress` と `Egress` が分かれて表示される場合は、**ポート2の両方をEnable**に設定します。

- `Ingress`：PS5がポート2を通してスイッチへ送るトラフィック
- `Egress`：スイッチがポート2を通してPS5へ送るトラフィック

![TL-SG108Eのポートミラーリング設定例](assets/setup/common/tl-sg108e-port-mirror-guide.svg)

この画像は入力する値を示す説明用イラストです。実際の画面は、ハードウェアバージョンやファームウェアによって異なる場合があります。

最後に **Apply**を選択します。

<!-- Screenshot: assets/setup/common/11-tplink-port-mirror-both.png
     Capture Monitoring > Port Mirror with Status=Enable, Mirroring Port=8,
     Mirrored Port=2, Mirrored Mode=Both. -->
<!-- Screenshot: assets/setup/common/12-tplink-port-mirror-directions.png
     Alternative UI: capture port 2 with Ingress=Enable and Egress=Enable,
     and Mirroring Port=8. -->

[TP-Link公式設定マニュアル - Configuring Port Mirror](https://static.tp-link.com/upload/manual/2025/202510/20251023/1910013735_Easy%20Smart%20Configuration%20Utility_UG_1022.pdf#page=48)には、実際のポートミラーリング設定画面がスクリーンショット付きで掲載されています。マニュアル上の44～45ページ（PDFの48～49ページ）で、`Both` 方式と `Ingress/Egress` 方式の両方を確認できます。

> [!CAUTION]
> ポート1はルーターにつながるアップリンクです。ポート1を `Mirrored/Source` に選ぶと、家庭内のほかの有線機器のトラフィックまで複製される可能性があります。PS5が接続されているポート2だけを選択してください。

### 8. PS5の有線LAN MACアドレスを確認する

PS5には、Wi-Fi用と有線LAN用で別々のMACアドレスがあります。PS5-P2PINFOには、必ず **有線LANのMACアドレス**を入力してください。

1. PS5のホーム画面右上にある **設定**を開きます。
2. **システム → システムソフトウェア → 本体情報**の順に進みます。
3. **MACアドレス（LANケーブル）**を書き留めます。
4. **MACアドレス（Wi-Fi）**は使用しません。

詳しいメニュー位置は、[PlayStation公式のPS5本体情報ガイド](https://www.playstation.com/ja-jp/support/hardware/playstation-system-software-application-version/)で確認できます。

<!-- Screenshot: assets/setup/ja/13-ps5-lan-mac.png
     Capture Console Information in Japanese. Mask console name, serial number and all real addresses. -->

### 9. キャプチャ用Ethernetアダプターを選択する

1. PS5-P2PINFOを起動し、**Config**タブを開きます。
2. **Choose Interface...**を選択します。
3. TL-SG108Eのポート8につながっているPCの物理Ethernetアダプターを選択します。
4. 選択した行が強調表示されていることを確認し、**Use Selected**を押します。

アプリケーション起動時にEthernetアダプターの候補が自動選択される場合がありますが、実際にポート8へ接続されているアダプターか、必ず確認してください。ケーブルを一度抜いてから挿し直し、Windows上で接続状態が変わるEthernetアダプターを探すと判別しやすくなります。

選択する名前の例は、`Realtek PCIe GbE Family Controller`、`Intel(R) Ethernet Controller`、`USB Gigabit Ethernet` です。次の項目は選択しないでください。

- Wi-FiまたはWirelessアダプター
- `NPF_Loopback` または `Adapter for loopback capture`
- Bluetoothアダプター
- VPN、Hyper-V、VMware、VirtualBoxなどの仮想アダプター

<!-- Screenshot: assets/setup/common/14-app-interface-selection.png
     Capture a physical Ethernet row selected and the Use Selected button. -->

### 10. PS5のMACアドレスを保存する

1. **Config**タブの **PS5 MAC Address**に、先ほど確認した有線LANのMACアドレスを入力します。
2. `AA:BB:CC:DD:EE:FF` のように、2文字ごとにコロンで区切る形式を推奨します。
3. **Promiscuous mode**を選択した状態にします。
4. そのほかの詳細設定は、最初は既定値のまま使用します。
5. **Save Config**を選択します。
6. **Log**タブに `Configuration saved.` と表示されることを確認します。

<!-- Screenshot: assets/setup/common/15-app-config.png
     Capture Config with the interface selected, a masked sample MAC,
     Promiscuous mode enabled and Save Config visible. -->

### 11. キャプチャを開始する

1. **Monitor**タブへ移動します。
2. ウィンドウ上部の **Start Capture**を選択します。
3. ステータスが `Ready` から `Capturing` に変わることを確認します。
4. PS5でArmored Coreのオンラインロビーまたはマッチに入ります。
5. P2Pトラフィックが検出されると、`Active peers` の数とモニター表が更新されます。
6. 終了するには **Stop**を選択します。

**Start Capture**を押したときにも、現在の設定は自動保存されます。**Save Config**は、キャプチャ開始前に保存結果を明示的に確認したい場合に使用できます。

<!-- Screenshot: assets/setup/common/16-app-capturing.png
     Capture Status=Capturing and Active peers. Mask any user-identifying values. -->

### トラブルシューティング

#### Start Captureが無効になっている

- Ethernetキャプチャインターフェースが選択されているか確認します。
- コロン、ハイフン、ピリオドを除いた16進数文字 `0–9`、`A–F` が12文字あるか確認します。例：`AA:BB:CC:DD:EE:FF`。
- Wi-Fi用ではなく、PS5の有線LAN用MACアドレスを入力したか確認します。

#### Start Captureを押すとアプリケーションが終了する、またはStart failedと表示される

- Npcapを最新バージョンで再インストールします。
- WinPcap API-compatible Modeが選択されているか確認します。
- Npcapを管理者専用モードでインストールした場合は、キャプチャ開始時のUAC要求を承認します。権限エラーが続く場合は、PS5-P2PINFOを **管理者として実行**します。
- **Log**タブの最後のエラーメッセージを確認します。

#### CapturingのままActive peersが0から変わらない

- PS5がWi-Fiではなく、TL-SG108Eのポート2へ有線接続されているか確認します。
- `Mirrored/Source = 2` と `Mirroring/Destination = 8` の順序が逆になっていないか確認します。
- `Both`、またはポート2の `Ingress + Egress` が両方とも有効か確認します。
- PCがポート8へ接続され、アプリケーションでその物理Ethernetアダプターを選択しているか確認します。
- 実際にP2P通信が発生するオンラインロビーまたはマッチへ入っているか確認します。

#### Pingが異常に高い、または表示されない

- 片方向だけをミラーリングすると、要求と応答の組み合わせを正しく特定できない場合があります。ポート2の送信・受信が両方ともミラーリングされているか確認します。
- ルーターのアップリンクであるポート1ではなく、PS5を直接接続したポート2だけを監視対象にします。

#### キャプチャ中にPCでインターネットを利用しにくい

PS5-P2PINFOのPingは、ミラーリングされたPS5のトラフィックから受動的に算出されるため、Pingを表示するだけであれば、監視PC自体のインターネット接続や2台目のアダプターは必要ありません。TL-SG108Eのポート8に接続したアダプターは、キャプチャ専用として使用してください。キャプチャ中にPCで別のpingテストを実行したり、通常のインターネット接続を利用したりする場合は、Wi-Fiまたは2台目の**予備Ethernetアダプター**を使用してください。キャプチャ用アダプターでは、Windowsのネットワークブリッジやインターネット接続の共有を有効にしないでください。

## ソースとライセンス

このリポジトリでは、リリースバイナリとドキュメントのみを配布します。PS5-P2PINFOのプロプライエタリなソースコードは公開しておらず、Rubicon Developersが所有するコードにはオープンソースライセンスを付与していません。Copyright © 2026 Rubicon Developers. All rights reserved.

インストーラーにはサードパーティ製コンポーネントも含まれており、各コンポーネントにはそれぞれのライセンスが引き続き適用されます。これはPS5-P2PINFO独自コードのプロプライエタリな位置付けを変更するものではありません。該当する著作権、ライセンスおよびソース入手先の案内については、[THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt)をご確認ください。

## 免責事項

本アプリケーションは独立したコミュニティツールであり、Sony Interactive Entertainment、FromSoftware、Bandai Namco Entertainmentとの提携または公式な承認を受けた製品ではありません。
