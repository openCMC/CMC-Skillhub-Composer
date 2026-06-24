# CMC Skill Hub — ChatGPT web install

Connect the CMC Skill Hub remote MCP service in ChatGPT web (custom MCP app, OAuth).
This file is for ChatGPT web only. Do not give local CLI, `mcp.json`, or terminal
commands. If you are a local agent, stop and tell the user to use the local install
file instead.

## Connection

- Connector name: `CMC Skill Hub`
- Remote MCP endpoint: `https://mcp.coinmarketcap.com/skill-hub/stream`
- Transport: Streamable HTTP / remote MCP
- Auth: OAuth — the CoinMarketCap authorization page asks for the API Key
- Verify tool: `find_skill`, query: `btc price`

## Steps

1. Open ChatGPT web -> `Apps` (`应用`).
2. Open app settings -> `Advanced settings` (`高级设置`) -> turn on
   `Developer mode` (`开发人员模式`).
3. Back in `Apps`, click `Create app` (`创建应用`).
4. Name: `CMC Skill Hub`. Description: `CMC Skill Hub`.
5. MCP server URL: `https://mcp.coinmarketcap.com/skill-hub/stream` (do not change it
   to `/sse` even if the placeholder shows one).
6. Authentication (`身份验证`): select `OAuth`.
7. Read the custom MCP risk notice; check `I understand and want to continue`
   (`我了解并希望继续`) only if the user confirms.
8. Click `Create` (`创建`).

If `Developer mode`, `Create app`, or a custom MCP app entry is not visible, stop and
tell the user the setup cannot be completed from this ChatGPT account — it depends on
plan, rollout, and admin/workspace permissions.

## Authentication

After `Create`, ChatGPT should redirect to a CoinMarketCap authorization page. Confirm
the page is on a CoinMarketCap-owned domain (e.g. `pro.coinmarketcap.com`) and the
redirect points back to a trusted ChatGPT connector URL (e.g.
`https://chatgpt.com/connector/oauth/...`), then have the user enter the API Key on
that page. If it does not redirect to a trusted CoinMarketCap page, stop and report the
exact screen.

## Enable & verify

Enable the `CMC Skill Hub` app from the tool/app menu, start a new chat, then ask it to
call `find_skill` with query `btc price`. Expect candidates such as
`daily_market_overview`, `crypto_macro_overview`, or `btc_cross_asset_correlation`.

_ChatGPT UI labels checked 2026-06; menu names may change in newer versions._
