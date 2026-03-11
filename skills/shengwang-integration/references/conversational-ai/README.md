# Shengwang Conversational AI Engine (ConvoAI)

Real-time AI voice agent. User speaks into an RTC channel, agent responds via ASR → LLM → TTS pipeline.

## How It Works

```
User Device ── audio ──► RTC Channel ──► ConvoAI Agent (ASR → LLM → TTS)
User Device ◄── audio ── RTC Channel ◄── ConvoAI Agent
```

- Agent is server-side only — managed via REST API, no client SDK
- Client uses RTC SDK (Web/Android/iOS) to join the channel
- `POST /join` makes the agent join the same RTC channel

## Auth

- REST calls: HTTP Basic Auth (`AGORA_CUSTOMER_KEY` + `AGORA_CUSTOMER_SECRET`)
- ConvoAI requires separate activation in [Shengwang Console](https://console.shengwang.cn/) — 403 without it
- The `token` field in `/join` is for the RTC channel, NOT for REST auth:
  - App Certificate not enabled → `""`
  - App Certificate enabled → generate via [token-server](../token-server/README.md)
- Credentials → [general/credentials-and-auth.md](../general/credentials-and-auth.md)

## Quick Start Docs

Fetch docs using the doc fetching script (see [doc-fetching.md](../doc-fetching.md)):

| Language | Command |
|----------|---------|
| Python / JS / curl | `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start"` |
| Go | `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start-go"` |
| Java | `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/convoai/restful/get-started/quick-start-java"` |

API endpoint index → [convoai-restapi.md](convoai-restapi.md)

## Sample Repos

For reference projects and starter layouts, use [sample-repos.md](sample-repos.md).

When the user wants help with web project structure or sample app organization:
- Pick the relevant entry from `sample-repos.md`
- Clone the repo on demand with `git clone --depth 1 <repo-url>`
- Inspect the current stack, folder map, key files, and env/config shape
- Prefer the repo as the primary reference when it contains the needed implementation pattern
- Fall back to doc fetching only when the repo does not contain useful guidance for the user's question
- Reuse the structural patterns that fit the user's project without copying sample-specific details blindly

Keep repo URLs in `sample-repos.md` only so future URL changes stay centralized.

## Generation Rules

Stable constraints that do NOT change with API updates. Always apply when generating code.

### Field Types (common pitfalls)
- `agent_rtc_uid`: STRING `"0"`, not int `0`
- `remote_rtc_uids`: array `["*"]`, not `"*"`
- `name`: unique per project — use `agent_{uuid[:8]}`
- `agent_rtc_uid` must not collide with any human participant's UID

### Create Agent (`POST /join`)
- `token`: `""` if no App Certificate; otherwise RTC token
- `agent_rtc_uid`: `"0"` for auto-assign
- `remote_rtc_uids`: `["*"]` unless user specifies UIDs

### Update Agent (`POST /update`)
- `llm.params` is FULLY REPLACED — always send complete object
- Only `token` and `llm` are updatable; everything else is immutable

### Terminology
- `agentId` in URL paths = `agent_id` in JSON bodies
- `/join` returns `agent_id` (snake_case); use it as path param

### Error Handling
- 409: extract existing `agent_id` or generate new name, retry
- 503/504: exponential backoff, max 3 retries
- Always parse `detail` + `reason` from error responses
- Full diagnosis → [common-errors.md](common-errors.md)

## Demo Projects

See [sample-repos.md](sample-repos.md) for the maintained ConvoAI sample registry.

## Docs Fallback

If fetch fails: https://doc.shengwang.cn/doc/convoai/restful/get-started/quick-start
