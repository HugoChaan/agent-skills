# Shengwang Resource Downloader

Downloads Agora SDKs, sample projects, and GitHub repos to the local workspace.

## Usage

```bash
python3 <path-to-this-dir>/scripts/downloader.py <url> [workspace_root_path]
```

- GitHub repos: `git clone --depth 1`
- File URLs (ZIP, TAR, etc.): download + auto-extract
- Output: `<workspace_root>/.tmp/<random>/`

Requires: `git`, `python3`, `requests` (`pip install requests`)

## Common Resources

| Resource | URL |
|----------|-----|
| AgoraDynamicKey (Token Builder, all languages) | `https://github.com/AgoraIO/Tools` |
| ConvoAI Go REST client | `https://github.com/AgoraIO-Community/agora-rest-client-go` |
| ConvoAI Java REST client | `https://github.com/AgoraIO-Community/agora-rest-client-java` |
| ConvoAI server sample | `https://github.com/Shengwang-Community/Conversational-AI-Server-Sample` |
| RTC Web sample | `https://github.com/AgoraIO/API-Examples-Web` |
| RTC Android/iOS sample | `https://github.com/AgoraIO/API-Examples` |

## Rules

- GitHub URLs must be repo root only — no branch or subdirectory paths
  - ✅ `https://github.com/AgoraIO-Community/agora-rest-client-go`
  - ❌ `https://github.com/AgoraIO-Community/agora-rest-client-go/tree/main/services`
- On any download failure: report the error, provide the URL for manual download, never silently skip
