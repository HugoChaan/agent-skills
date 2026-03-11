# ConvoAI REST API Reference

Endpoint index with local documentation. For the full API overview, see [convoai-restapi/index.mdx](convoai-restapi/index.mdx).

## Base URL

```
https://api.agora.io/cn/api/conversational-ai-agent/v2/projects/{AGORA_APP_ID}
```

## Authentication

HTTP Basic Auth — see [README.md](README.md#auth) and [general/credentials-and-auth.md](../general/credentials-and-auth.md).

## Endpoints

| Method | Path | Local Doc |
|--------|------|-----------|
| POST | `/join` | [start-agent.md](convoai-restapi/start-agent.md) |
| POST | `/agents/{agentId}/leave` | [stop-agent.md](convoai-restapi/stop-agent.md) |
| POST | `/agents/{agentId}/update` | [agent-update.md](convoai-restapi/agent-update.md) |
| GET | `/agents/{agentId}` | [query-agent-status.md](convoai-restapi/query-agent-status.md) |
| GET | `/agents` | [get-agent-list.md](convoai-restapi/get-agent-list.md) |
| POST | `/agents/{agentId}/speak` | [agent-speak.md](convoai-restapi/agent-speak.md) |
| POST | `/agents/{agentId}/interrupt` | [agent-interrupt.md](convoai-restapi/agent-interrupt.md) |
| GET | `/agents/{agentId}/history` | [get-history.md](convoai-restapi/get-history.md) |

All endpoints index: [convoai-restapi/index.mdx](convoai-restapi/index.mdx)

## Error Response Format

All non-200 responses:
```json
{
  "detail": "error description",
  "reason": "ErrorCode"
}
```

Error diagnosis → [common-errors.md](common-errors.md)

## Docs Fallback

If fetch fails, use README.md Generation Rules + ask the user to verify against:
https://doc.shengwang.cn/doc/convoai/restful/get-started/quick-start
