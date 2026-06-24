# CMC Skill Hub — Claude Desktop install

Connect the CMC Skill Hub MCP server in Claude Desktop via the `mcp-remote` stdio
bridge. This file is for Claude Desktop only; if you are on another platform, fetch
the matching file from `README.md`.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: Streamable HTTP via `mcp-remote` (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Install

Claude Desktop local MCP config launches local stdio processes, so bridge to the
remote endpoint with `mcp-remote`. Merge into:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "cmc-skill-hub": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote@latest",
        "https://mcp.coinmarketcap.com/skill-hub/stream",
        "--header",
        "X-CMC-MCP-API-KEY:${CMC_MCP_API_KEY}"
      ],
      "env": { "CMC_MCP_API_KEY": "<API_KEY>" }
    }
  }
}
```

Notes:

- In `--header`, write `X-CMC-MCP-API-KEY:${CMC_MCP_API_KEY}` with no space around
  the colon; `mcp-remote` expects the `Header:value` form and expands the `env`
  placeholder.
- Do not add `--transport sse-only`. If a client still uses `/skill-hub/sse` as a
  compatibility URL, do not infer legacy SSE; `mcp-remote` connects http-first.

## Timeout

Claude Desktop local MCP config has no documented per-tool execution timeout field.
`mcp-remote --auth-timeout` only covers the OAuth callback, not tool execution. If
long-running tools get cut off, it is a Claude Desktop / bridge limitation, not a
server-side MCP failure.

## Reload & verify

Fully quit and restart Claude Desktop, then run `find_skill(query="btc price")`.
Expect candidates such as `daily_market_overview` or `btc_cross_asset_correlation`.
