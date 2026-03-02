# AGENTS.md

This repository contains AI agent skills for Shengwang (Agora) platform integration.

## How to Use

1. Start from [skills/SKILL.md](skills/SKILL.md) — it is the root router for all requests
2. The root router decides whether to run intake (needs analysis) or route directly to a product skill
3. Each product skill follows: confirm credentials → fetch docs via MCP → generate code → validate

## Repository Structure

```
skills/
├── SKILL.md                   # Root router (start here)
├── mcp-tools.md               # MCP tool usage guide (internal)
├── intake/                    # Needs analysis → product routing
├── general/                   # Credentials, REST auth (shared)
├── integrate-shengwang-conversational-ai/  # ConvoAI
├── integrate-shengwang-rtc/               # RTC SDK
├── integrate-shengwang-rtm/               # RTM
├── integrate-shengwang-cloud-recording/   # Cloud Recording
├── implement-shengwang-token-on-server/   # Token generation
└── resource-downloader/                   # SDK/sample downloads
```

## MCP Integration

These skills work best with the Agora Doc MCP server (`https://doc-mcp.shengwang.cn/mcp`).
Skills provide behavioral guidance and workflow; MCP provides up-to-date API documentation.
See [skills/mcp-tools.md](skills/mcp-tools.md) for tool usage patterns.

## Rules

- Always start routing from `skills/SKILL.md`
- Never hardcode credentials — use environment variables
- Fetch MCP quick start docs before generating code for any product
- When MCP is unavailable, use local references + fallback URLs, and tell the user to verify
