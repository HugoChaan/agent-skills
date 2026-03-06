# ConvoAI Authentication

> Credential setup & REST auth → [general/credentials-and-auth.md](../general/credentials-and-auth.md)
> RTC token generation → [implement-shengwang-token-on-server](../implement-shengwang-token-on-server/README.md)

## ConvoAI-Specific Rules

### URL Structure

`AGORA_APP_ID` goes in the **URL path**, not the auth header:
```
https://api.agora.io/cn/api/conversational-ai-agent/v2/projects/{AGORA_APP_ID}
```

### RTC Channel Token in /join

The `token` field in the `/join` payload is for the **RTC channel**, not for REST auth:

| App Certificate | `token` value |
|----------------|--------------|
| Not enabled | `""` (empty string) |
| Enabled | Generate via RTC Token Builder — see [implement-shengwang-token-on-server](../implement-shengwang-token-on-server/README.md) |

If token expires mid-session → call `POST /agents/{agentId}/update` with a new token.

## Enabling ConvoAI Service

ConvoAI must be explicitly enabled for your project before any API calls will work.

If you get HTTP **403**:
1. ConvoAI service not enabled → Agora Console → enable ConvoAI for your project
2. Wrong `AGORA_CUSTOMER_KEY` / `AGORA_CUSTOMER_SECRET` → regenerate in Console
3. `AGORA_APP_ID` mismatch between URL path and credentials project

Full error reference → use MCP to fetch the relevant endpoint doc, and see [troubleshooting/common-errors.md](../troubleshooting/common-errors.md)
