# CMC Skill Hub — MCP install (startup)

Per-platform instructions for connecting the **CMC Skill Hub** MCP server. Each
file is self-contained: fetch the one that matches your environment and follow it.

## How an agent uses this folder

1. Detect the current environment (a local CLI/IDE agent, or a web-hosted chat AI).
2. Pick the matching file below.
3. Fetch it — local agents use `curl`; web AIs read the URL — and follow it.
4. If you cannot fetch it, ask the user to paste the file contents.

Raw URL pattern:

```text
https://raw.githubusercontent.com/openCMC/CMC-Skillhub-Composer/<ref>/cmc-skillhub-composer/reference/startup/<file>
```

`<ref>` is a branch, tag, or commit (for example `main`).

## Files

### Local agents / IDEs / CLI (HTTP header auth)

| Platform | File |
|---|---|
| Claude Code | `claude-code.md` |
| Claude Desktop | `claude-desktop.md` |
| Cursor | `cursor.md` |
| Codex | `codex.md` |
| OpenClaw | `openclaw.md` |

### Web-hosted chat AI (OAuth / custom connector)

| Platform | File |
|---|---|
| ChatGPT web | `chatgpt-web.md` |
| Claude Chat web / Claude Cowork | `claude-web.md` |

## Connection (shared by every file)

| Field | Value |
|---|---|
| MCP Endpoint | `https://mcp.coinmarketcap.com/skill-hub/stream` |
| MCP Server ID | `cmc-skill-hub` |
| Transport | Streamable HTTP (NOT legacy SSE) |
| Local auth | HTTP header `X-CMC-MCP-API-KEY: <API_KEY>` |
| Web auth | OAuth (enter the API Key on the CoinMarketCap authorization page) |
| Verify | `find_skill(query="btc price")` |

Security, every platform: never put the API Key in the URL; merge into existing
MCP config rather than overwriting.
