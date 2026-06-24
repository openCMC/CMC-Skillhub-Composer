# CMC Skill Hub — Codex install

Connect the CMC Skill Hub MCP server in Codex. This file is for Codex CLI only; if
you are on another platform, fetch the matching file from `README.md`.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: `streamable_http` (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Install

Merge into `~/.codex/config.toml`:

```toml
[mcp_servers.cmc-skill-hub]
url = "https://mcp.coinmarketcap.com/skill-hub/stream"
tool_timeout_sec = 300
startup_timeout_sec = 20

[mcp_servers.cmc-skill-hub.http_headers]
X-CMC-MCP-API-KEY = "<API_KEY>"
```

Notes:

- Do not use an SSE stdio bridge for this endpoint.
- `codex mcp add` can register a streamable_http URL, but it has no flag for a custom
  header like `X-CMC-MCP-API-KEY` (only bearer-token auth), so set the header by editing
  `config.toml` directly. (Checked Codex CLI 0.128.0, 2026-06; re-verify on newer versions.)
- `tool_timeout_sec = 300` is the per-server MCP tool timeout; `startup_timeout_sec`
  only covers server startup.

## Reload & verify

Start a new Codex session. Verify with `codex mcp get cmc-skill-hub`; it must show
`transport: streamable_http`, `http_headers: X-CMC-MCP-API-KEY=...`, and
`tool_timeout_sec: 300`. Then run `find_skill(query="btc price")`. Expect candidates
such as `daily_market_overview` or `btc_cross_asset_correlation`.
