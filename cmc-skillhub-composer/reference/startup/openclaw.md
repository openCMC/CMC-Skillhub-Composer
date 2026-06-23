# CMC Skill Hub — OpenClaw install

Connect the CMC Skill Hub MCP server in OpenClaw. This file is for OpenClaw only; if
you are on another platform, fetch the matching file from `README.md`.

## Reply language

Reply in the language the user writes in. Keep config keys, paths, commands,
headers, and URLs verbatim — do not translate them.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: `streamable-http` (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Install

```bash
openclaw mcp set cmc-skill-hub '{"url":"https://mcp.coinmarketcap.com/skill-hub/stream","transport":"streamable-http","connectionTimeoutMs":300000,"headers":{"X-CMC-MCP-API-KEY":"<API_KEY>"}}'
openclaw gateway restart
```

OpenClaw stores the API Key in MCP config; treat it as sensitive.

## Timeout

`connectionTimeoutMs = 300000` covers the remote Streamable HTTP connection. Current
public OpenClaw docs do not confirm a separate per-tool execution timeout; if a tool
call can still be cut off, report it as a client limitation.

## Reload & verify

After `openclaw gateway restart`, start a fresh agent session if the current one does
not list the new tools, then run `find_skill(query="btc price")`. Expect candidates
such as `daily_market_overview` or `btc_cross_asset_correlation`.

## Report back

Platform used, that `openclaw mcp set` plus `gateway restart` ran, the connection
timeout (300000ms), and whether `find_skill` verification passed.
