# PS5-P2PINFO

**English** | [한국어](README.ko.md) | [日本語](README.ja.md)

PS5-P2PINFO is a Windows monitoring tool for Armored Core peer traffic captured from a mirrored PS5 LAN port.

## Download

Download the latest Windows x64 installer, `PS5-P2PINFO.exe`, from [GitHub Releases](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest).

This `.exe` is an installer built with Inno Setup, not a portable application file. It installs the application, creates the selected shortcuts, and adds a standard Windows uninstall entry. The application is self-contained, so a separate .NET installation is not required.

The installer is currently unsigned, so Windows SmartScreen may show a warning. Verify the SHA-256 checksum published with each release before running it.

## Features

- Pilot-name monitoring
- Ping and RTT jitter measurements
- Recent RTT spike diagnostics
- Packet-gap diagnostics
- Configurable display and capture settings

## Supported environment

- Windows 10 or Windows 11, x64
- A managed switch configured for port mirroring
- Both PS5 traffic directions mirrored to the monitoring PC
- [Npcap](https://npcap.com/#download) installed with **WinPcap API-compatible Mode** enabled
- Administrator privileges for installation and packet capture

Windows ARM64 is not currently provided as a separate build.

## Installation and setup

1. Download `PS5-P2PINFO.exe` from the latest release.
2. Install Npcap with WinPcap API-compatible Mode enabled.
3. Run the installer and select the installation folder and shortcut options.
4. Connect the PS5 and monitoring PC to the managed switch.
5. Mirror both ingress and egress traffic for the PS5 port to the monitoring PC's port.
6. Launch PS5-P2PINFO and select the mirrored Ethernet adapter.
7. Enter the PS5 MAC address and save the configuration.
8. Select **Start Capture**.

## Source and license

This repository distributes release binaries and documentation only. The source code is not published, and no open-source license is granted. Copyright © 2026 Rubicon Developers. All rights reserved.

## Disclaimer

This is an independent community tool and is not affiliated with or endorsed by Sony Interactive Entertainment, FromSoftware, or Bandai Namco Entertainment.
