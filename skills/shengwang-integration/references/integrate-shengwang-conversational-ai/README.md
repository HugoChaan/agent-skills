# Shengwang Conversational AI Engine (ConvoAI)

## Routing Guardrails

> Intake skip/require logic is defined in [SKILL.md](../../SKILL.md) (root router).
> If you arrived here without a structured spec and the request is vague,
> redirect to [intake](../../intake/README.md).

- From intake with structured spec → skip to Workflow step 2.
- Direct operation with enough details (e.g. "stop agent xxx") → proceed directly.

---

## Resource Lookup

| Priority | Source | Use for |
|----------|--------|---------|
| 1 | [convoai-restapi.md](convoai-restapi.md) | Stable API structure: endpoints, lifecycle, MCP URI index |
| 2 | MCP ConvoAI REST API docs (per-endpoint URIs in convoai-restapi.md) | Full request/response schemas, vendor params (AUTHORITATIVE) |
| 3 | Generation Rules (below) | Field type gotchas, naming, error handling — things the spec can't express |

### Quick Start URIs (MCP)

| Language | URI |
|----------|-----|
| Python / JS / curl | `docs://default/convoai/restful/get-started/quick-start` |
| Go | `docs://default/convoai/restful/get-started/quick-start-go` |
| Java | `docs://default/convoai/restful/get-started/quick-start-java` |

**MCP fallback:** If unavailable, use Generation Rules below + fallback URL, and tell the user to verify against https://doc.shengwang.cn/doc/convoai/restful/get-started/quick-start

---

## Workflow

### Step 1: Confirm Credentials & Service Activation

Need `AGORA_APP_ID`, `AGORA_CUSTOMER_KEY`, `AGORA_CUSTOMER_SECRET`.
Missing? → [general/credentials-and-auth.md](../general/credentials-and-auth.md)

> **ConvoAI requires separate activation.** The user must enable ConvoAI for their project in [Shengwang Console](https://console.shengwang.cn/), otherwise API calls return 403.
> Details → [authentication.md](authentication.md#enabling-convoai-service)

Need Go/Java SDK or AgoraDynamicKey? → [resource-downloader](../resource-downloader/README.md)

### Step 2: Fetch Quick Start via MCP (MANDATORY)

Call `get-doc-content` with the URI matching the user's dev language (table above).
Read the returned doc fully before writing any code.

### Step 3: Generate Code

1. Quick start doc as primary code reference
2. Consult MCP API endpoint docs for detailed field definitions (see Resource Lookup table)
3. Apply Generation Rules (below)
4. Apply user's intake spec (language, LLM, TTS vendor, etc.)

### Step 4: Validate

- [ ] Credentials from env vars, never hardcoded
- [ ] `agent_rtc_uid` is string `"0"`, not int `0`
- [ ] `remote_rtc_uids` is `["*"]`, not `"*"`
- [ ] `name` has random suffix (`agent_{uuid[:8]}`) to avoid 409
- [ ] Error handling covers 409 and 503/504
- [ ] TTS/ASR params match vendor schema in OpenAPI spec

---

## Quick Reference

**Architecture:**
`User Voice → RTC Channel → ASR → LLM → TTS → RTC Channel → User hears AI`
Agent joins RTC channel via REST API (no client-side SDK for the agent).
Details → [architecture.md](architecture.md)

**Auth:** HTTP Basic Auth on all ConvoAI REST calls:
`Authorization: Basic base64("{CUSTOMER_KEY}:{CUSTOMER_SECRET}")`
Details → [authentication.md](authentication.md)

**Base URL:** `https://api.agora.io/cn/api/conversational-ai-agent/v2/projects/{AGORA_APP_ID}`

---

## Generation Rules

Stable constraints that do NOT change with API updates. Always apply.

### Field Types
- `agent_rtc_uid`: STRING `"0"`, not int `0`
- `remote_rtc_uids`: array `["*"]`, not `"*"`
- `name`: unique per project — use `agent_{uuid[:8]}`

### Create Agent
- `token`: `""` if no App Certificate; otherwise RTC token builder
- `agent_rtc_uid`: `"0"` for auto-assign
- `remote_rtc_uids`: `["*"]` unless user specifies UIDs

### Update Agent
- `llm.params` is FULLY REPLACED — always send complete object
- Only `token` and `llm` are updatable; everything else is immutable

### Terminology
- `agentId` in URL paths (`/agents/{agentId}/leave`) = `agent_id` in JSON bodies
- `/join` returns `agent_id` (snake_case); use it as path param

### Error Handling
- 409: extract existing `agent_id` or generate new name, retry
- 503/504: exponential backoff, max 3 retries
- Always parse `detail` + `reason` from error responses
- Diagnosis → [common-errors.md](common-errors.md)
