# CMC Skill Hub — Claude Code install

Connect the CMC Skill Hub MCP server in Claude Code. This file is for Claude Code
only; if you are on another platform, fetch the matching file from `README.md`.

## Reply language

Reply in the language the user writes in. Keep config keys, paths, commands,
headers, and URLs verbatim — do not translate them.

## Connection

- MCP Endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- MCP Server ID: `cmc-skill-hub`
- Transport: Streamable HTTP (NOT legacy SSE)
- Auth header: `X-CMC-MCP-API-KEY: <API_KEY>`
- Verify: `find_skill(query="btc price")`

## Security

- Put the key in request headers as `X-CMC-MCP-API-KEY`; never in the URL.
- Prefer user-level config. Merge into existing `mcpServers`; if `cmc-skill-hub`
  already exists, update only that entry. Do not overwrite other servers.
- If `<API_KEY>` is a placeholder, ask the user for the real key first.
- Claude Code may echo configured headers; redact them and never print the literal key.

## Install (preferred: CLI)

```bash
export CMC_MCP_API_KEY="<API_KEY>"
set -o pipefail
claude mcp add --transport http --scope user cmc-skill-hub "https://mcp.coinmarketcap.com/skill-hub/stream" \
  --header "X-CMC-MCP-API-KEY: ${CMC_MCP_API_KEY}" 2>&1 \
  | sed -E 's/(X-CMC-MCP-API-KEY[^[:alnum:]]+)[^" }]+/\1<redacted>/g'
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

Merge into project `.mcp.json`, then remind the user not to commit a key:

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

## Report back

Platform used, files/commands changed, MCP server id, whether the tool timeout was
set to 300s, and whether `find_skill` verification passed.
