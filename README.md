# Shengwang (Agora) Skills

[![中文](https://img.shields.io/badge/lang-中文-red.svg)](README_CN.md)

Reusable skills for AI coding agents building with the [Shengwang (Agora)](https://shengwang.cn/) platform. These skills help agents accurately integrate, configure, and debug Agora products.

## Available Skills

| Skill | Product | Description |
|-------|---------|-------------|
| [conversational-ai](skills/shengwang-integration/references/conversational-ai/README.md) | ConvoAI | AI voice agent lifecycle: create/stop/update/query. Supports Go, Java, Python |
| [rtc](skills/shengwang-integration/references/rtc/README.md) | RTC SDK | Real-time audio/video calls. Web, Android, iOS, Flutter, and more |
| [rtm](skills/shengwang-integration/references/rtm/README.md) | RTM | Real-time messaging, signaling, presence |
| [cloud-recording](skills/shengwang-integration/references/cloud-recording/README.md) | Cloud Recording | Server-side recording of RTC sessions |
| [token-server](skills/shengwang-integration/references/token-server/README.md) | Token Server | Server-side token generation (AccessToken2) |
| [general](skills/shengwang-integration/references/general/credentials-and-auth.md) | General | Credential management, REST auth patterns |
| [intake](skills/shengwang-integration/intake/README.md) | Routing | Needs analysis → product recommendation → route to product module |

## Quick Start

### Installation Methods

#### Skills CLI

Install with CLI:

```bash
npx skills add Shengwang-Community/skills
```

This is the most direct installation method. After installation, restart the session or refresh the skills list according to your coding agent's instructions.

#### Claude Code Plugin Marketplace

Run the following command in Claude Code:

```bash
plugin marketplace add Shengwang-Community/skills
```
#### OpenClaw

Use `ClawHub install + sync` for installation and updates. Use `install` for the first setup, then `sync` for subsequent updates.

```bash
openclaw skill install <shengwang-placeholder>
openclaw skill sync <shengwang-placeholder>
```

### 2. Configure MCP (Recommended)

These skills are designed to work alongside the [Agora Doc MCP Server](https://doc-mcp.shengwang.cn). Skills provide behavioral guidance and workflow; MCP provides up-to-date API documentation.

Add to your MCP configuration:

```json
{
  "mcpServers": {
    "agora-docs": {
      "type": "sse",
      "url": "https://doc-mcp.shengwang.cn/mcp"
    }
  }
}
```

> Skills work without MCP too — they fall back to local reference docs and external doc links.

### 3. Start Using

Describe your needs to the agent — skills trigger automatically:

- "I want to build an AI voice assistant" → intake analysis → ConvoAI + RTC integration
- "Generate an RTC token in Go" → Token Server module
- "How to implement video calls on Web" → RTC SDK module
- "Download the ConvoAI Go SDK" → Resource Downloader

## How It Works

```
User Request
   │
   ▼
skills/shengwang-integration/SKILL.md (entry point)
   │
   ├─ Vague request → intake (needs analysis → product recommendation)
   │                      │
   │                      ▼
   │                 Product module (code generation)
   │
   └─ Clear request → Route directly to product module
```

The entry point (`skills/shengwang-integration/SKILL.md`) determines whether the request is specific enough:
- Clear and actionable → route directly to the matching product module
- Vague or missing details → run intake to collect requirements first, then route

Each product module follows a consistent workflow: confirm credentials → fetch latest docs via MCP → generate code → validate.

## Repository Structure

```
shengwang-skills/
├── README.md                  # This file
├── AGENTS.md                  # Agent entry point instructions
├── CLAUDE.md                  # → AGENTS.md
├── CONTRIBUTING.md            # Contribution guidelines
├── scripts/
│   └── validate-skills.sh     # Link and frontmatter validation
├── tests/
│   └── eval-cases.md          # Evaluation test cases
└── skills/
    └── shengwang-integration/     # The skill (agentskills.io standard)
        ├── SKILL.md               # Entry point and router (only SKILL.md)
        ├── intake/                # Needs analysis and product routing
        └── references/            # All product modules and shared knowledge
            ├── mcp-tools.md           # MCP tool usage guide
            ├── general/               # Credentials, REST auth
            ├── conversational-ai/                  # ConvoAI
            ├── rtc/                               # RTC SDK
            ├── rtm/                               # RTM
            ├── cloud-recording/                   # Cloud Recording
            ├── token-server/                      # Token generation
            └── mcp-tools.md               # MCP tool usage guide
```

## Design Philosophy

- Behavior over knowledge: skills teach agents *how to approach* integration; MCP provides *specific APIs*
- Single responsibility: each module does one thing
- Progressive disclosure: SKILL.md serves as navigation; detailed content lives in `references/` and module `README.md` files
- Explicit failure paths: every module defines error handling
- Eval-driven iteration: validate changes against `tests/eval-cases.md`

## Validation

```bash
bash scripts/validate-skills.sh
```

Checks all SKILL.md frontmatter format and markdown link validity.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Key requirements:

- The root skill has a `SKILL.md` with YAML frontmatter (name, description, metadata.author, metadata.version)
- Sub-modules use `README.md` (no frontmatter needed)
- Directory names use kebab-case
- Detailed docs go in `references/`; keep SKILL.md and README.md concise
- Run `bash scripts/validate-skills.sh` before submitting

## Links

- [Shengwang Console](https://console.shengwang.cn/)
- [Agora Docs (CN)](https://doc.shengwang.cn/)
- [GitHub](https://github.com/AgoraIO-Community)
