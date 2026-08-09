# PS5-P2PINFO

[English](README.md) | [한국어](README.ko.md) | **日本語**

PS5-P2PINFOは、ミラーリングされたPS5の有線LANトラフィックを利用して、Armored CoreのP2P接続状態を監視するWindowsアプリケーションです。

## 画面例

![PS5-P2PINFO モニタリング画面の例](assets/example.png)

この画像ではPS5のMACアドレスをマスクしています。

## ダウンロード

[GitHub Releases](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest)から、最新のWindows x64用インストーラー `PS5-P2PINFO-Setup.exe` をダウンロードしてください。

この `.exe` はポータブル実行ファイルではなく、Inno Setupで作成されたインストーラーです。アプリケーションのインストール、選択したショートカットの作成、Windowsの標準アンインストール項目の登録を行います。.NETを含むself-contained形式のため、.NETを別途インストールする必要はありません。

現在、インストーラーにはデジタルコード署名がないため、Windows SmartScreenの警告が表示される場合があります。実行前に、各リリースで公開されているSHA-256チェックサムを確認してください。

## 主な機能

- パイロット名のモニタリング
- PingおよびRTT Jitterの測定
- 最近のRTT Spike診断
- Packet Gap診断
- 表示およびキャプチャ設定の調整

## 関連ポリシーおよび利用規約

適用されるポリシーおよび利用規約は、国または地域によって異なります。この日本語版では、日本に居住するユーザーおよび日本のPlayStationアカウントに適用される公式リンクを案内しています。ほかの国または地域で利用する場合は、その地域の最新規約をご確認ください。ゲームまたはサービス内で個別に提示される規約も適用される場合があります。

PS5-P2PINFOは、適用される法律、プラットフォームポリシーおよびサービス利用規約を尊重し、遵守することを目的として設計されています。

- [PlayStation Network利用規約 – 日本](https://www.playstation.com/ja-jp/legal/psn-terms-of-service/)
- [PlayStationソフトウェアアプリケーション使用許諾契約 – 日本](https://www.playstation.com/ja-jp/legal/software-eula/)
- [PS5用システムソフトウェア使用許諾契約 – 日本](https://www.playstation.com/ja-jp/legal/ps5-ssla/)

## 対応環境

- Windows 10またはWindows 11 x64
- ポートミラーリングに対応したルーターまたはマネージドスイッチ
- PS5の送受信トラフィックが双方向とも監視PCへミラーリングされている環境
- **WinPcap API-compatible Mode**を有効にしてインストールした[Npcap](https://npcap.com/#download)
- インストールおよびパケットキャプチャのための管理者権限

現在、Windows ARM64向けの個別ビルドは提供していません。

## インストールと設定

1. 最新リリースから `PS5-P2PINFO-Setup.exe` をダウンロードします。
2. WinPcap API-compatible Modeを有効にしてNpcapをインストールします。
3. インストーラーを実行し、インストール先とショートカットのオプションを選択します。
4. PS5と監視PCを、ポートミラーリングに対応したルーターまたはマネージドスイッチに接続します。
5. PS5ポートの受信・送信トラフィックを監視PCのポートへミラーリングします。
6. PS5-P2PINFOを起動し、ミラーリングトラフィックを受信するEthernetアダプターを選択します。
7. PS5のMACアドレスを入力して設定を保存します。
8. **Start Capture**を選択します。

## ソースとライセンス

このリポジトリでは、リリースバイナリとドキュメントのみを配布します。ソースコードは公開しておらず、オープンソースライセンスも付与していません。Copyright © 2026 Rubicon Developers. All rights reserved.

## 免責事項

本アプリケーションは独立したコミュニティツールであり、Sony Interactive Entertainment、FromSoftware、Bandai Namco Entertainmentとの提携または公式な承認を受けた製品ではありません。
