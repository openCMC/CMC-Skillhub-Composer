# CMC Skill Hub — Cursor install

Connect the CMC Skill Hub MCP server in Cursor. This file is for Cursor only; if you
are on another platform, fetch the matching file from `README.md`.

## Reply language

Reply in the language the user writes in. Keep config keys, paths, commands,
headers, and URLs verbatim — do not translate them.

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

Prefer global `~/.cursor/mcp.json`; merge, do not overwrite:

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

Only use project `.cursor/mcp.json` if the user explicitly asks; then remind them
not to commit the key.

## Reload & verify

Reload the Cursor window (or restart Cursor), then run `find_skill(query="btc price")`.
Expect candidates such as `daily_market_overview` or `btc_cross_asset_correlation`.

## Report back

Platform used, UI vs JSON, config file changed, and whether `find_skill`
verification passed.
