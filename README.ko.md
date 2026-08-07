# PS5-P2PINFO

[English](README.md) | **한국어** | [日本語](README.ja.md)

PS5-P2PINFO는 미러링된 PS5 유선 LAN 트래픽을 이용해 Armored Core의 P2P 연결 상태를 모니터링하는 Windows 프로그램입니다.

## 예시 화면

![PS5-P2PINFO 모니터링 예시](assets/example.png)

이 이미지의 PS5 MAC 주소는 마스킹되어 있습니다.

## 다운로드

[GitHub Releases](https://github.com/Rubicon-Developers/PS5-P2P-INFO/releases/latest)에서 최신 Windows x64 설치 프로그램인 `PS5-P2PINFO-Setup.exe`를 다운로드하세요.

이 `.exe` 파일은 포터블 실행 파일이 아니라 Inno Setup으로 제작된 설치 프로그램입니다. 애플리케이션을 설치하고, 선택한 바로가기를 생성하며, Windows의 표준 앱 제거 항목을 등록합니다. .NET을 포함한 self-contained 방식이므로 별도로 .NET을 설치할 필요는 없습니다.

현재 설치 파일에는 디지털 코드 서명이 적용되지 않아 Windows SmartScreen 경고가 표시될 수 있습니다. 실행하기 전에 각 릴리스에 제공되는 SHA-256 체크섬을 확인하세요.

## 주요 기능

- 파일럿 이름 모니터링
- Ping 및 RTT Jitter 측정
- 최근 RTT Spike 진단
- Packet Gap 진단
- 화면 및 캡처 설정 조정

## 지원 환경

- Windows 10 또는 Windows 11 x64
- 포트 미러링을 지원하는 공유기 또는 관리형 스위치
- PS5 송신·수신 양방향 트래픽이 모니터링 PC로 미러링된 환경
- **WinPcap API-compatible Mode**를 활성화하여 설치한 [Npcap](https://npcap.com/#download)
- 프로그램 설치와 패킷 캡처를 위한 관리자 권한

현재 Windows ARM64용 별도 빌드는 제공하지 않습니다.

## 설치 및 설정

1. 최신 릴리스에서 `PS5-P2PINFO-Setup.exe`를 다운로드합니다.
2. WinPcap API-compatible Mode를 활성화하여 Npcap을 설치합니다.
3. 설치 프로그램을 실행하고 설치 폴더와 바로가기 옵션을 선택합니다.
4. PS5와 모니터링 PC를 포트 미러링을 지원하는 공유기 또는 관리형 스위치에 연결합니다.
5. PS5 포트의 수신·송신 트래픽을 모두 모니터링 PC 포트로 미러링합니다.
6. PS5-P2PINFO를 실행하고 미러링 트래픽을 받는 Ethernet 어댑터를 선택합니다.
7. PS5 MAC 주소를 입력하고 설정을 저장합니다.
8. **Start Capture**를 선택합니다.

## 소스 및 라이선스

이 저장소는 릴리스 바이너리와 문서만 배포합니다. 소스 코드는 공개하지 않으며 오픈 소스 라이선스를 부여하지 않습니다. Copyright © 2026 Rubicon Developers. All rights reserved.

## 면책 고지

이 프로그램은 독립적인 커뮤니티 도구이며 Sony Interactive Entertainment, FromSoftware 또는 Bandai Namco Entertainment와 제휴하거나 공식 승인을 받은 제품이 아닙니다.
