# CMC Skill Hub — Claude Chat web / Claude Cowork install

Connect the CMC Skill Hub remote MCP service in Claude Chat web or Claude Cowork
(custom connector, OAuth). This file is for Claude Chat web / Cowork only. Do not use
Claude Desktop local config, local JSON files, or terminal commands. If you are a
local agent or Claude Desktop, stop and tell the user to use the matching local file.

## Connection

- Connector name: `CMC Skill Hub`
- Remote MCP endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- Transport: Streamable HTTP / remote MCP
- Auth: OAuth — the CoinMarketCap authorization page asks for the API Key
- Verify tool: `find_skill`, query: `btc price`

Check first:

- Custom connectors work on Free, Pro, Max, Team, and Enterprise. Free may allow only
  one custom connector.
- The remote MCP server must be reachable from Anthropic's cloud, not only from the
  user's machine, VPN, or private network.
- Claude.ai connector calls can time out after 300 seconds.

## Steps (personal Free / Pro / Max)

1. Open Claude Chat web or Cowork -> `Customize` -> `Connectors`.
2. Click `+` -> `Add custom connector`.
3. Name: `CMC Skill Hub`.
4. URL: `https://mcp.coinmarketcap.com/skill-hub/stream`.
5. Open `Advanced settings` only if Claude asks for an OAuth Client ID / Secret, or
   the CMC Skill Hub owner provided them; otherwise leave those fields blank.
6. Click `Add`, then click `Connect` to authenticate.

## Steps (Team / Enterprise)

1. You must be Owner or Primary Owner; otherwise ask one to add the org connector.
2. `Organization settings` -> `Connectors` -> `Add`.
3. Hover `Custom` -> select `Web`. URL: `https://mcp.coinmarketcap.com/skill-hub/stream`.
   Click `Add`.
4. Members: `Customize` -> `Connectors` -> find `CMC Skill Hub` (`Custom` label) ->
   `Connect`.

## Authentication

After `Connect`, Claude opens an OAuth flow to a CoinMarketCap-owned domain (e.g.
`pro.coinmarketcap.com`) with a trusted callback such as
`https://claude.ai/api/mcp/auth_callback`; have the user enter the key on that page. If
Claude asks for an OAuth Client ID / Secret the owner did not provide, stop — do not
invent credentials. If you see `Couldn't reach the MCP server`, a timeout, or missing
auth metadata, stop and report it.

## Enable & verify

Enable `CMC Skill Hub` for the conversation via the chat `+` menu -> `Connectors`;
reload the page or start a new chat. Then ask Claude to call `find_skill` with query
`btc price`. Expect candidates such as `daily_market_overview`,
`crypto_macro_overview`, or `btc_cross_asset_correlation`.
