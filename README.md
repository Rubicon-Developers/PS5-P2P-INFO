# AC P2P Info

AC P2P Info is a Windows monitoring tool for Armored Core peer traffic captured from a mirrored PS5 LAN port.

## Download

Download the latest `AcP2PInfoSetup.exe` from [GitHub Releases](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest).

The installer is currently unsigned, so Windows SmartScreen may show a warning. Verify the SHA-256 checksum published with each release before running the installer.

## Features

- Displays pilot name, ping, RTT jitter, recent RTT spike, and packet-gap diagnostics.
- Stores application settings, including the configured PS5 MAC address, locally in `config.json`.

## Requirements

- Windows 10 or Windows 11, x64
- A managed switch configured for port mirroring
- Both PS5 traffic directions mirrored to the monitoring PC
- [Npcap](https://npcap.com/#download) installed with **WinPcap API-compatible Mode** enabled
- Administrator privileges for packet capture and installation

## Setup

1. Connect the PS5 and monitoring PC to the managed switch.
2. Mirror both ingress and egress traffic for the PS5 port to the PC's monitoring port.
3. Install Npcap with WinPcap API-compatible Mode enabled.
4. Install and launch AC P2P Info.
5. Select the mirrored Ethernet adapter.
6. Enter the PS5 MAC address and save the configuration.
7. Select **Start Capture**.

## Source and license

This repository distributes release binaries and documentation only. The source code is not published, and no open-source license is granted. Copyright © 2026 Rubicon Developers. All rights reserved.

## Disclaimer

This is an independent community tool and is not affiliated with or endorsed by Sony Interactive Entertainment, FromSoftware, or Bandai Namco Entertainment.
