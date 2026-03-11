# ConvoAI Detail Collection

Reached from [intake](README.md) after ConvoAI is identified as the primary product.
This file is for ConvoAI-specific follow-up only.

## Language Detection

Detect the user's language from their most recent message:
- If the user writes in **Chinese** → use the **ZH** prompts below
- If the user writes in **English** (or any other language) → use the **EN** prompts below

Maintain the detected language consistently throughout the entire intake flow.

## Prerequisites

Before starting, the user should have:
- Completed the main kickoff intake
- A clear use case description
- Platform / client-stack context already collected if relevant
- Backend language already collected if relevant

## Questions

Use a friendly follow-up flow:
- Ask one question at a time
- Keep prompts short
- Skip anything the user already answered
- Only block on credentials when they are required to continue

Defaults policy:
- ASR vendor may default to `fengming`
- ASR language may be inferred from the use case or default to `zh-CN` / `en-US`
- LLM may default to `deepseek`
- TTS may default to `bytedance`

For ASR, ASR language, LLM, and TTS, offer the recommended default in plain language once.
If the user does not want to customize, accept the default and continue.
Do not force provider-by-provider confirmation before implementation.

Only credentials are normally blocking in this section.
ASR vendor and ASR language should not block progress unless the user has a
specific requirement that materially affects implementation.

Ask **one at a time** only when needed. Skip any question the user already answered during main intake.
Doc index status is already determined by the main intake — do not re-check here.

### Q1 — Credentials & App Certificate

**ZH:**
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

**EN:**
> "Do you have an Agora account and project credentials?"
>
> Required:
> - `AppID` — project identifier
> - `Customer Key` + `Customer Secret` — REST API auth
> - `App Certificate` — is it enabled?
>
> Options:
> - A. All ready, App Certificate is enabled
> - B. All ready, App Certificate is not enabled (or unsure)
> - C. Have an account but haven't created a project yet
> - D. Don't have an account yet

**If A** → Record `certificate = enabled`, token generation needed later.

| | Prompt |
|---|--------|
| ZH | "App Certificate 已开启，ConvoAI 创建 agent 时需要传入 RTC Token。我会在后续帮你生成 Token，需要用到 `AGORA_APP_CERTIFICATE` 环境变量。" |
| EN | "App Certificate is enabled. ConvoAI requires an RTC Token when creating an agent. I'll help you generate one later — you'll need the `AGORA_APP_CERTIFICATE` env var." |

**If B** → Record `certificate = not enabled`, token = empty string.

| | Prompt |
|---|--------|
| ZH | "如果后续在 Console 开启了 App Certificate，就需要改为传入 Token，否则 agent 会加入频道失败。" |
| EN | "If you enable App Certificate later in Console, you'll need to start passing a Token, otherwise the agent will fail to join the channel." |

**If C or D** → Direct to https://console.shengwang.cn/ and pause until ready.

### Q2 — LLM

**ZH:**
> "LLM 先用默认的 DeepSeek 可以吗？如果你想指定别的，我再切换。"
> - A. 阿里云（aliyun）
> - B. 字节跳动（bytedance）
> - C. 深度求索（deepseek）
> - D. 腾讯（tencent）
> - E. OpenAI（openai）
> - F. 用默认的就行（deepseek）

**EN:**
> "Is it okay to start with the default LLM, DeepSeek? If you want a different one, I can switch."
> - A. Alibaba Cloud (aliyun)
> - B. ByteDance (bytedance)
> - C. DeepSeek (deepseek)
> - D. Tencent (tencent)
> - E. OpenAI (openai)
> - F. Use the default (deepseek)

**Default:** deepseek

### Q3 — TTS

**ZH:**
> "TTS 先用默认的火山引擎可以吗？如果你有指定供应商，我再改。"
> - A. 字节跳动 / 火山引擎（bytedance）
> - B. 微软（microsoft）
> - C. MiniMax（minimax）
> - D. 阿里 CosyVoice（cosyvoice）
> - E. 腾讯（tencent）
> - F. 阶跃星辰（stepfun）
> - G. 用默认的就行（bytedance）

**EN:**
> "Is it okay to start with the default TTS, ByteDance? If you prefer another provider, I can change it."
> - A. ByteDance / Volcengine (bytedance)
> - B. Microsoft (microsoft)
> - C. MiniMax (minimax)
> - D. Alibaba CosyVoice (cosyvoice)
> - E. Tencent (tencent)
> - F. StepFun (stepfun)
> - G. Use the default (bytedance)

**Default:** bytedance (Volcengine TTS)

### Q4 — ASR Vendor

**ZH:**
> "ASR 先用默认的凤鸣可以吗？如果你想指定别的，我再切换。"
> - A. 声网凤鸣（fengming）— 默认
> - B. 腾讯（tencent）
> - C. 微软（microsoft）
> - D. 科大讯飞（xfyun）
> - E. 科大讯飞大模型（xfyun_bigmodel）
> - F. 科大讯飞方言（xfyun_dialect）
> - G. 用默认的就行（fengming）

**EN:**
> "Is it okay to start with the default ASR, Fengming? If you want a different one, I can switch."
> - A. Agora Fengming (fengming) — default
> - B. Tencent (tencent)
> - C. Microsoft (microsoft)
> - D. iFlytek (xfyun)
> - E. iFlytek BigModel (xfyun_bigmodel)
> - F. iFlytek Dialect (xfyun_dialect)
> - G. Use the default (fengming)

**Default:** fengming

### Q5 — ASR Language

Choose the recommended default from the use case:
- English use case -> `en-US`
- Chinese or unspecified use case -> `zh-CN`

**ZH:**
> "识别语言先用默认的 [zh-CN / en-US] 可以吗？如果你想换成别的语言，我再改。"
> - A. 中文（zh-CN，支持中英混合）
> - B. 英文（en-US）
> - C. 其他（请说明）
> - D. 用默认的就行

**EN:**
> "Is it okay to start with the default recognition language, [zh-CN / en-US]? If you want another language, I can change it."
> - A. Chinese (zh-CN, supports Chinese-English mix)
> - B. English (en-US)
> - C. Other (please specify)
> - D. Use the default

**Default:** `en-US` for clearly English scenarios, otherwise `zh-CN`

---

## Output: Structured Spec

**ZH:**
```
ConvoAI 需求规格
─────────────────────────────
凭证状态：        [已就绪 / 需先创建]
App Certificate： [已开启 / 未开启]
Token：           [需要生成 / 空字符串]
ASR：             [fengming (default applied) / tencent / microsoft / xfyun / xfyun_bigmodel / xfyun_dialect]
ASR 语言：        [zh-CN (default applied) / en-US (default applied) / ja-JP / ko-KR / ...]
LLM：             [aliyun / bytedance / deepseek (default applied) / tencent / openai]
TTS：             [bytedance (default applied) / minimax / tencent / microsoft / cosyvoice / stepfun]
─────────────────────────────
```

**EN:**
```
ConvoAI Spec
─────────────────────────────
Credentials:      [Ready / Need to create]
App Certificate:  [Enabled / Not enabled]
Token:            [Need to generate / Empty string]
ASR:              [fengming (default applied) / tencent / microsoft / xfyun / xfyun_bigmodel / xfyun_dialect]
ASR Language:     [zh-CN (default applied) / en-US (default applied) / ja-JP / ko-KR / ...]
LLM:              [aliyun / bytedance / deepseek (default applied) / tencent / openai]
TTS:              [bytedance (default applied) / minimax / tencent / microsoft / cosyvoice / stepfun]
─────────────────────────────
```

The backend language should come from the main kickoff summary rather than this file.

## Defaults

| Field | Default | Notes (ZH) | Notes (EN) |
|-------|---------|------------|------------|
| App Certificate | Not enabled | 如果用户不确定，按未开启处理，提醒后续开启需改传 Token | If user is unsure, treat as not enabled; remind them to pass Token if enabled later |
| ASR vendor | `fengming` | 声网凤鸣 ASR | Agora Fengming ASR |
| ASR language | `zh-CN` / `en-US` | 默认中文场景用 `zh-CN`；英文场景用 `en-US`；如用户接受推荐值，按 default applied 记录 | Use `zh-CN` for Chinese or unspecified scenarios and `en-US` for clearly English scenarios; mark accepted recommendation as default applied |
| LLM vendor | `deepseek` | 如用户选默认则使用此值 | Used when user picks default |
| TTS vendor | `bytedance` | 火山引擎 TTS | Volcengine TTS |

> ASR/TTS/LLM valid values come from the /join API docs — see [convoai-restapi/start-agent.md](../references/conversational-ai/convoai-restapi/start-agent.md) for the /join schema and vendor params. Do not invent values.

## Route After Collection

Pass the structured spec to [conversational-ai](../references/conversational-ai/README.md).
The product module will use the spec to fetch the right docs and generate code.

Key routing hints:
- Dev = Go → run `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start-go"`
- Dev = Java → run `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start-java"`
- Dev = Python/curl → run `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start"`
- App Certificate = Enabled → also run [token-server](../references/token-server/README.md)
- If fetch fails → use Generation Rules + fallback URL
