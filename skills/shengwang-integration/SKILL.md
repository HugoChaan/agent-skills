---
name: shengwang-integration
description: |
  Integrate Shengwang (Agora) products: ConvoAI voice agents, RTC audio/video,
  RTM messaging, Cloud Recording, and token generation. Use when the user
  mentions Shengwang, Agora, 声网, ConvoAI, RTC, RTM, voice agent, AI agent,
  video call, live streaming, recording, token, or any Agora product task.
license: MIT
metadata:
  author: shengwang
  version: "1.0.0"
---

# Shengwang Integration

## Routing Rules

### Step 1: Check if intake can be skipped

Skip [intake](intake/README.md) and route directly ONLY when ALL of these are true:
- User names a specific operation (e.g. "stop agent xxx", "generate a token", "download SDK")
- User provides enough technical details to act immediately (channel name, agent ID, language, etc.)
- The request maps unambiguously to exactly one skill in the table below

Examples that SKIP intake:
- "帮我停掉 agent_abc123" → [integrate-shengwang-conversational-ai](references/integrate-shengwang-conversational-ai/README.md)
- "生成一个 RTC token，Go 语言" → [implement-shengwang-token-on-server](references/implement-shengwang-token-on-server/README.md)
- "error 403 是什么意思" → [integrate-shengwang-conversational-ai/troubleshooting](references/integrate-shengwang-conversational-ai/common-errors.md)
- "下载 ConvoAI Go SDK" → [resource-downloader](references/resource-downloader/README.md)

Examples that MUST go through intake:
- "我想做一个 AI 客服" → needs analysis, go to intake
- "帮我接入 ConvoAI" → product identified but details missing, go to intake
- "我想做视频通话 + AI 助手" → multi-product, go to intake
- "voice bot" / "AI agent" → vague, go to intake

### Step 2: Route

| User intent | Route to |
|-------------|----------|
| New request, vague, or missing details | [intake](intake/README.md) |
| Credentials, AppID, REST auth | [general](references/general/credentials-and-auth.md) |
| Download SDK, sample project, Token Builder, GitHub repo | [resource-downloader](references/resource-downloader/README.md) |
| Generate Token, token server, AccessToken2, RTC/RTM auth | [implement-shengwang-token-on-server](references/implement-shengwang-token-on-server/README.md) |
| ConvoAI operation (with details already known) | [integrate-shengwang-conversational-ai](references/integrate-shengwang-conversational-ai/README.md) |
| RTC SDK integration | [integrate-shengwang-rtc](references/integrate-shengwang-rtc/README.md) |
| RTM messaging / signaling | [integrate-shengwang-rtm](references/integrate-shengwang-rtm/README.md) |
| Cloud Recording | [integrate-shengwang-cloud-recording](references/integrate-shengwang-cloud-recording/README.md) |

## Links

- Console: https://console.shengwang.cn/
- Docs (CN): https://doc.shengwang.cn/
- GitHub: https://github.com/AgoraIO-Community
