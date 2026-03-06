# Agora Doc MCP Tools

Internal guide for the model. Describes how to use the Agora Doc MCP server
to fetch up-to-date documentation during skill execution.

**MCP endpoint:** `https://doc-mcp.shengwang.cn/mcp`

## Tools

| Tool | Input | Returns | When to use |
|------|-------|---------|-------------|
| `get-doc-content` | `{"uri": "docs://..."}` | Full markdown content | Read a specific doc (preferred) |
| `search-docs` | `{"query": "keyword"}` | List of matching doc URIs | Find docs when URI is unknown |
| `list-docs` | `{"category": "...", "limit": 20}` | All docs in a category | Browse available docs |

## Preferred Approach: Direct URI

When the doc URI is known (each product README lists its MCP URIs), call `get-doc-content` directly:

```
get-doc-content {"uri": "docs://default/convoai/restful/get-started/quick-start"}
```

Do NOT search first if you already have the URI from a product README.

## Fallback: Search Then Read

When the URI is unknown, search first:

```
Step 1: search-docs {"query": "convoai <topic>"}
        → returns [{uri: "docs://...", text: "..."}, ...]

Step 2: get-doc-content {"uri": "docs://..."}
        → returns full doc content
```

## When to Call MCP

**Always call for:**
- API field details, request/response formats
- Vendor configurations (TTS, ASR)
- Error codes and their meanings
- Any content that may change with documentation updates

**Do NOT call for:**
- Generation rules (field types, naming conventions) — stable, in skill files
- Auth patterns — stable, in [general/credentials-and-auth.md](general/credentials-and-auth.md)
- Workflow steps — stable, in skill files
