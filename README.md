# PS5-P2PINFO

**English** | [한국어](README.ko.md) | [日本語](README.ja.md)

PS5-P2PINFO is a Windows monitoring tool for Armored Core peer traffic captured from a mirrored PS5 LAN port.

## Example screen

![PS5-P2PINFO monitoring example](assets/example.png)

The PS5 MAC address is masked in this image.

## Download

Download the latest Windows x64 installer, `PS5-P2PINFO-Setup.exe`, from [GitHub Releases](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest).

This `.exe` is an installer built with Inno Setup, not a portable application file. It installs the application, creates the selected shortcuts, and adds a standard Windows uninstall entry. The application is self-contained, so a separate .NET installation is not required.

The installer is currently unsigned, so Windows SmartScreen may show a warning. Verify the SHA-256 checksum published with each release before running it.

## Features

- Pilot-name monitoring
- Ping and RTT jitter measurements
- Recent RTT spike diagnostics
- Packet-gap diagnostics
- Configurable display and capture settings

## Policies and Terms

Applicable policies and terms vary by country or region. The official links below are organized by country or region; please review the section that applies to your place of residence and PlayStation account. Additional terms presented within a game or service may also apply.

PS5-P2PINFO is designed with the intention of respecting and complying with applicable laws, platform policies, and terms of service.

### South Korea

- [PlayStation Terms of Service](https://www.playstation.com/ko-kr/legal/psn-terms-of-service/)
- [PlayStation Software Application EULA](https://www.playstation.com/ko-kr/legal/software-eula/)
- [PS5 System Software Licence Agreement](https://www.playstation.com/ko-kr/legal/ps5-ssla/)
- [Bandai Namco Entertainment Korea Terms of Use](https://www.bandainamcoent.asia/ko-kr/terms-of-use)

### Japan

- [PlayStation Network Terms of Service](https://www.playstation.com/ja-jp/legal/psn-terms-of-service/)
- [PlayStation Software Application EULA](https://www.playstation.com/ja-jp/legal/software-eula/)
- [PS5 System Software Licence Agreement](https://www.playstation.com/ja-jp/legal/ps5-ssla/)

### United States and the Americas

- [PlayStation Terms of Service](https://www.playstation.com/en-us/legal/terms-of-service/)
- [PlayStation Software Application EULA](https://www.playstation.com/en-us/legal/software-eula/)
- [PS5 System Software Licence Agreement](https://www.playstation.com/en-us/legal/ps5-ssla/)
- [Bandai Namco Entertainment America Terms of Service](https://www.bandainamcoent.com/legal/bnea-tos-online-services)

## Supported environment

- Windows 10 or Windows 11, x64
- A router or managed switch that supports port mirroring
- Both PS5 traffic directions mirrored to the monitoring PC
- [Npcap](https://npcap.com/#download) installed with **WinPcap API-compatible Mode** enabled
- Administrator privileges for installation and packet capture

Windows ARM64 is not currently provided as a separate build.

## Installation and setup

1. Download `PS5-P2PINFO-Setup.exe` from the latest release.
2. Install Npcap with WinPcap API-compatible Mode enabled.
3. Run the installer and select the installation folder and shortcut options.
4. Connect the PS5 and monitoring PC to the router or managed switch that supports port mirroring.
5. Mirror both ingress and egress traffic for the PS5 port to the monitoring PC's port.
6. Launch PS5-P2PINFO and select the mirrored Ethernet adapter.
7. Enter the PS5 MAC address and save the configuration.
8. Select **Start Capture**.

## Source and license

This repository distributes release binaries and documentation only. The source code is not published, and no open-source license is granted. Copyright © 2026 Rubicon Developers. All rights reserved.

## Disclaimer

This is an independent community tool and is not affiliated with or endorsed by Sony Interactive Entertainment, FromSoftware, or Bandai Namco Entertainment.
