---
name: convoai-intake
description: |
  Collects ConvoAI-specific implementation details after the main intake has
  identified ConvoAI as the target product. Outputs a structured spec that
  drives code generation in integrate-shengwang-conversational-ai/SKILL.md.
---

# ConvoAI Detail Collection

Reached from [intake/SKILL.md](../SKILL.md) after ConvoAI is identified as the primary product.

## Prerequisites

Before starting, the user should have:
- Confirmed ConvoAI is the right product (via main intake)
- A clear use case description

## Questions

**Fast-path rule:** If the user's initial description already contains 3+ of the following
(credentials status, LLM choice, TTS choice, dev language), skip individual questions.
Instead, generate the structured spec directly from what they said, fill defaults for anything
missing, and present it for confirmation.

Ask **one at a time** only when needed. Skip any question the user already answered during main intake.

### Q1 — Credentials & App Certificate

> "你有 Agora 账号和项目凭证吗？"
>
> 需要以下信息：
> - `AppID` — 项目标识
> - `Customer Key` + `Customer Secret` — REST API 认证
> - `App Certificate` — 是否已开启？
>
> 选择：
> - A. 都准备好了，App Certificate 已开启
> - B. 都准备好了，App Certificate 未开启（或不确定）
> - C. 有账号但还没创建项目
> - D. 还没有账号

**If A** → 记录 `证书状态 = 已开启`，后续需要生成 Token。
提示用户：
> "App Certificate 已开启，ConvoAI 创建 agent 时需要传入 RTC Token。
> 我会在后续帮你生成 Token，需要用到 `AGORA_APP_CERTIFICATE` 环境变量。"

**If B** → 记录 `证书状态 = 未开启`，token 传空字符串即可。
提示用户：
> "如果后续在 Console 开启了 App Certificate，就需要改为传入 Token，否则 agent 会加入频道失败。"

**If C or D** → direct to https://console.agora.io/ and pause until ready.

### Q2 — LLM

> "你打算用哪个 LLM？"
> - A. 阿里云（aliyun）
> - B. 字节跳动（bytedance）
> - C. 深度求索（deepseek）
> - D. 腾讯（tencent）
> - E. 还没决定，用推荐的就行

**Default:** aliyun

### Q3 — TTS

> "你打算用哪个 TTS（语音合成）？"
> - A. 字节跳动 / 火山引擎（bytedance）
> - B. 微软（microsoft）
> - C. MiniMax（minimax）
> - D. 阿里 CosyVoice（cosyvoice）
> - E. 腾讯（tencent）
> - F. 阶跃星辰（stepfun）
> - G. 还没决定，用推荐的就行

**Default:** bytedance（火山引擎 TTS）

### Q4 — Development language

> "你用什么语言开发服务端？"
> - A. Go
> - B. Java
> - C. Python / JavaScript / curl

### Q5 — MCP 状态

> "你是否已安装 Agora Doc MCP Server？（用于获取最新文档）"
> - A. 已安装
> - B. 没有 / 不确定

**If B** → 提示用户：
> "建议安装 Agora Doc MCP Server 以获取最新 API 文档，在 MCP 配置中添加：
> ```json
> {
>   "mcpServers": {
>     "agora-docs": {
>       "type": "sse",
>       "url": "https://doc-mcp.shengwang.cn/mcp"
>     }
>   }
> }
> ```
> 没有 MCP 也可以继续，我会使用本地参考文档。"

记录 MCP 状态，影响后续代码生成时的文档获取策略。

---

## Output: Structured Spec

```
ConvoAI 需求规格
─────────────────────────────
凭证状态：        [已就绪 / 需先创建]
App Certificate： [已开启 / 未开启]
Token：           [需要生成 / 空字符串]
ASR：             [fengming (default) / tencent / microsoft / xfyun / xfyun_bigmodel / xfyun_dialect]
LLM：             [aliyun / bytedance / deepseek / tencent]
TTS：             [bytedance (default) / minimax / tencent / microsoft / cosyvoice / stepfun]
开发语言：        [Go / Java / Python/curl]
MCP 状态：        [已安装 / 未安装]
─────────────────────────────
```

## Defaults

| Field | Default | Notes |
|-------|---------|-------|
| App Certificate | 未开启 | 如果用户不确定，按未开启处理，提醒后续开启需改传 Token |
| ASR vendor | `fengming` | 声网凤鸣 ASR，默认 zh-CN |
| ASR language | `zh-CN` | 中文（支持中英混合） |
| LLM vendor | `deepseek` | 需用户提供 LLM url (OpenAI 兼容) |
| TTS vendor | `bytedance` | 火山引擎 TTS |
| MCP | 未安装 | 降级到本地 OpenAPI spec + fallback URL |

> ASR/TTS/LLM 可选值均来自 [convoai-restapi.yaml](../integrate-shengwang-conversational-ai/references/convoai-restapi.yaml)，不可自行编造。

## Route After Collection

Pass the structured spec to [integrate-shengwang-conversational-ai/SKILL.md](../integrate-shengwang-conversational-ai/SKILL.md),
skipping questions already answered.

| Detail | Routing hint |
|--------|-------------|
| Dev = Go | ConvoAI SKILL.md → MCP `get-doc-content {"uri": "docs://default/convoai/restful/get-started/quick-start-go"}` |
| Dev = Java | ConvoAI SKILL.md → MCP `get-doc-content {"uri": "docs://default/convoai/restful/get-started/quick-start-java"}` |
| Dev = Python/curl | ConvoAI SKILL.md → MCP `get-doc-content {"uri": "docs://default/convoai/restful/get-started/quick-start"}` |
| App Certificate = 已开启 | [implement-shengwang-token-on-server/SKILL.md](../implement-shengwang-token-on-server/SKILL.md) to generate RTC Token |
| Needs Go/Java SDK | [resource-downloader/SKILL.md](../resource-downloader/SKILL.md) to download REST client SDK |
| Needs Token Builder | [resource-downloader/SKILL.md](../resource-downloader/SKILL.md) to download AgoraDynamicKey |
| MCP = 未安装 | Use local OpenAPI spec + Generation Rules, add fallback URL in output |
