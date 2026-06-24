# CMC Skill Hub — Cursor install

Connect the CMC Skill Hub MCP server in Cursor. This file is for Cursor only; if you
are on another platform, fetch the matching file from `README.md`.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: Streamable HTTP (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Install (preferred: UI)

Cursor Settings -> MCP -> add server:

- Name: `cmc-skill-hub`
- Type: `Streamable HTTP` (some versions label it `streamable-http`)
- URL: `https://mcp.coinmarketcap.com/skill-hub/stream`
- Header: `X-CMC-MCP-API-KEY: <API_KEY>`

## Install (manual JSON)

Merge into global `~/.cursor/mcp.json` (or project `.cursor/mcp.json`):

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

Reload the Cursor window (or restart Cursor), then run `find_skill(query="btc price")`.
Expect candidates such as `daily_market_overview` or `btc_cross_asset_correlation`.
