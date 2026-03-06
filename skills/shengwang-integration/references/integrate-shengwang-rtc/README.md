# Shengwang RTC SDK

Real-time audio/video communication SDK. Foundation layer for most Agora products.

## What It Does

- 1v1 / group audio & video calls
- Live streaming (host publishes, audience subscribes)
- Audio-only or video+audio
- Cross-platform: Web, Android, iOS, macOS, Windows, Flutter, React Native, Electron, Unity

## Core Flow

1. Initialize Agora engine with `AGORA_APP_ID`
2. Join channel with token (or empty string if no App Certificate)
3. Publish local audio/video tracks
4. Subscribe to remote tracks
5. Leave channel and destroy engine on exit

## Auth

- `AGORA_APP_ID` required
- If App Certificate enabled → need RTC token from server, see [implement-shengwang-token-on-server](../implement-shengwang-token-on-server/README.md)
- Credentials setup → [general/credentials-and-auth.md](../general/credentials-and-auth.md)

## Quick Start Docs (MCP)

| Platform | MCP URI |
|----------|---------|
| Web (JS) | `docs://default/rtc/javascript/get-started/quick-start` |
| Android | `docs://default/rtc/android/get-started/quick-start` |
| iOS | `docs://default/rtc/ios/get-started/quick-start` |
| Flutter | `docs://default/rtc/flutter/get-started/quick-start` |
| React Native | `docs://default/rtc/react-native/get-started/quick-start` |
| Electron | `docs://default/rtc/electron/get-started/quick-start` |

## Demo Projects

| Platform | Repo |
|----------|------|
| Web | https://github.com/AgoraIO/API-Examples-Web |
| Android / iOS | https://github.com/AgoraIO/API-Examples |

## Docs Fallback

If MCP is unavailable: https://doc.shengwang.cn/doc/rtc/javascript/get-started/quick-start
