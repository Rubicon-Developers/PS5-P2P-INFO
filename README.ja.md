# AC P2P Info

[English](README.md) | [한국어](README.ko.md) | **日本語**

AC P2P Infoは、ミラーリングされたPS5の有線LANトラフィックを利用して、Armored CoreのP2P接続状態を監視するWindowsアプリケーションです。

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

## 対応環境

- Windows 10またはWindows 11 x64
- ポートミラーリングに対応したマネージドスイッチ
- PS5の送受信トラフィックが双方向とも監視PCへミラーリングされている環境
- **WinPcap API-compatible Mode**を有効にしてインストールした[Npcap](https://npcap.com/#download)
- インストールおよびパケットキャプチャのための管理者権限

現在、Windows ARM64向けの個別ビルドは提供していません。

## インストールと設定

1. 最新リリースから `PS5-P2PINFO-Setup.exe` をダウンロードします。
2. WinPcap API-compatible Modeを有効にしてNpcapをインストールします。
3. インストーラーを実行し、インストール先とショートカットのオプションを選択します。
4. PS5と監視PCをマネージドスイッチに接続します。
5. PS5ポートの受信・送信トラフィックを監視PCのポートへミラーリングします。
6. AC P2P Infoを起動し、ミラーリングトラフィックを受信するEthernetアダプターを選択します。
7. PS5のMACアドレスを入力して設定を保存します。
8. **Start Capture**を選択します。

## ソースとライセンス

このリポジトリでは、リリースバイナリとドキュメントのみを配布します。ソースコードは公開しておらず、オープンソースライセンスも付与していません。Copyright © 2026 Rubicon Developers. All rights reserved.

## 免責事項

本アプリケーションは独立したコミュニティツールであり、Sony Interactive Entertainment、FromSoftware、Bandai Namco Entertainmentとの提携または公式な承認を受けた製品ではありません。
