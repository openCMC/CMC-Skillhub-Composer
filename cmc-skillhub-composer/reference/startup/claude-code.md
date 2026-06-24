# CMC Skill Hub — Claude Code install

Connect the CMC Skill Hub MCP server in Claude Code. This file is for Claude Code
only; if you are on another platform, fetch the matching file from `README.md`.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: Streamable HTTP (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Install (preferred: CLI)

```bash
claude mcp add --transport http --scope user cmc-skill-hub "https://mcp.coinmarketcap.com/skill-hub/stream" \
  --header "X-CMC-MCP-API-KEY: <API_KEY>"
```

## Timeout

Merge into user-level `~/.claude/settings.json`:

```json
{
  "env": {
    "MCP_TOOL_TIMEOUT": "300000",
    "MCP_TIMEOUT": "60000"
  }
}
```

`MCP_TOOL_TIMEOUT` covers long-running tool calls; `MCP_TIMEOUT` only covers MCP
server startup. Restart Claude Code after changing these.

## Manual JSON (only if the CLI is unavailable)

Merge into project `.mcp.json`:

```json
{
  "mcpServers": {
    "cmc-skill-hub": {
      "type": "http",
      "url": "https://mcp.coinmarketcap.com/skill-hub/stream",
      "headers": { "X-CMC-MCP-API-KEY": "<API_KEY>" }
    }
  }
}
```

## Reload & verify

Restart Claude Code (start a new session), then run `find_skill(query="btc price")`.
Expect candidates such as `daily_market_overview`, `crypto_macro_overview`, or
`btc_cross_asset_correlation`.
