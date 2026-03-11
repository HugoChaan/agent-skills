# ConvoAI REST API Reference

Endpoint index with documentation URIs. For full request/response schemas, fetch using the doc fetching script (see [doc-fetching.md](../doc-fetching.md)).

## Base URL

```
https://api.agora.io/cn/api/conversational-ai-agent/v2/projects/{AGORA_APP_ID}
```

## Authentication

HTTP Basic Auth — see [README.md](README.md#auth) and [general/credentials-and-auth.md](../general/credentials-and-auth.md).

## Endpoints

To fetch any endpoint doc, run: `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "<doc_uri>"`

| Method | Path | Doc URI |
|--------|------|---------|
| POST | `/join` | `docs://default/convoai/restful/convoai/operations/start-agent` |
| POST | `/agents/{agentId}/leave` | `docs://default/convoai/restful/convoai/operations/stop-agent` |
| POST | `/agents/{agentId}/update` | `docs://default/convoai/restful/convoai/operations/agent-update` |
| GET | `/agents/{agentId}` | `docs://default/convoai/restful/convoai/operations/query-agent-status` |
| GET | `/agents` | `docs://default/convoai/restful/convoai/operations/get-agent-list` |
| POST | `/agents/{agentId}/speak` | `docs://default/convoai/restful/convoai/operations/agent-speak` |
| POST | `/agents/{agentId}/interrupt` | `docs://default/convoai/restful/convoai/operations/agent-interrupt` |
| GET | `/agents/{agentId}/history` | `docs://default/convoai/restful/convoai/operations/get-history` |

All endpoints index: `docs://default/convoai/restful/convoai/operations`

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
