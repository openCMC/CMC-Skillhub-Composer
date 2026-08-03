---
name: cmc-skillhub-composer
description: Discovers and runs CMC Crypto Skill Hub services through its MCP server (find_skill, execute_skill) and renders clean, chat-ready Markdown research. Use for crypto market and BTC/ETH analysis, ETF flows, onchain token and memecoin scans, derivatives and perp positioning, liquidations, portfolio exposure and PnL attribution, trading-cost comparison, crypto macro regime, U.S. equity index, sector and single-stock research, U.S. macro and rates context, prediction and event market analytics, and cross-asset reads between crypto, equities, DXY, gold, and rates. Standalone U.S. equity, U.S. macro, and prediction-market questions are in scope. Not for tasks outside markets.
license: Apache-2.0
---

# CMC Crypto Skill Hub

Use this skill when the user asks for crypto market, BTC, ETH, ETF, derivatives, onchain, token, portfolio, or scanner research; for U.S. equity index, sector, theme, or single-stock research; for U.S. macro and rates context; for prediction and event market analytics; or for cross-asset reads between crypto, equities, and macro.

CMC Crypto Skill Hub is a remote service registry exposed through an MCP server with two tools: `find_skill` (discover services and read their `input_schema`) and `execute_skill` (run one). These are platform services, not local files — discover and run them through the MCP tools; never try to read or execute a service yourself.

## MCP tool names

This skill uses two tools from the connected CMC Crypto Skill Hub MCP server: `find_skill` (discover services and read their `input_schema`) and `execute_skill` (run one).

Do not assume a fixed server id. Across clients and environments the same service is registered under different ids — for example `cmc-skill-hub`, `crypto-skill-hub`, or a `*-beta` id — and its tools may surface under different names, such as `mcp__cmc-skill-hub__find_skill` in Claude Code or `cmc-skill-hub:find_skill` elsewhere. Call the tools by whatever name the connected server actually exposes for `find_skill` and `execute_skill`.

If both a production and a `*-beta` server are connected, prefer the production (non-beta) one. Do not use app or connector aliases when a CMC Crypto Skill Hub server is available. If none is connected, do not guess — use another MCP namespace only after verifying it exposes the same `find_skill` and `execute_skill` tools.

## When not to use

For tasks outside markets (weather, email, coding, general knowledge), do not call CMC Crypto Skill Hub; answer with another capability.

Standalone U.S. equity, U.S. macro, and prediction-market questions **are** in scope — they no longer need a crypto framing. Coverage grows over time, so when a market question plausibly fits, call `find_skill` and let an empty result decide rather than declining up front.

## Operating principle

Use the smallest set of services that answers the request well. Prefer one broad service when it covers the requested lenses; use several only when the user asks for separate lenses one service does not cover (for example ETF demand plus cross-asset behavior plus derivatives structure). Do not run an extra service just to add volume.

Treat CMC Crypto Skill Hub as the primary CMC-derived source. Use external sources only when the user asks for latest news, verification, or official context, or when a service is partial, stale, low-confidence, unavailable, or errors. Label external context separately from CMC-derived findings.

## Execution steps

For each selected service:

1. Call `find_skill(query=...)` to confirm the `unique_name` and `input_schema`.
2. Validate planned parameters against `input_schema`.
3. If a required field is missing, stop and ask the user for it before executing.
4. Call `execute_skill(unique_name=..., parameters=...)` once for that service. `parameters` must be a JSON object, not a JSON string.
5. Do not silently retry a failed run. If the only selected service fails, report the exact error and stop. If a supplemental service fails but other evidence exists, disclose the failure and lower confidence.

Do not expose tool traces, schema dumps, or raw wrapper structures in the final answer.

After a successful run, pick the report template with the Output Template Router below, then render. If the user asks for a different format, honor it while preserving status, confidence, missing-data warnings, and key numbers.

## Which service to run

- Daily crypto market overview → the daily market overview service.
- BTC versus equities, DXY, rates, gold, or oil → the BTC cross-asset correlation service.
- BTC ETF or institutional demand → the BTC ETF institutional demand service.
- Broad crypto macro regime → the crypto macro overview service.
- Macro news affecting BTC and equities → a macro news or cross-asset service.
- U.S. equity index level, session facts, or trailing index returns → the U.S. equity index snapshot service.
- U.S. equity sector or theme leadership and rotation → the U.S. equity sector rotation service. Do not substitute a crypto sector-rotation service; they cover different universes and `find_skill` may rank them close together.
- Onchain scanner requests → inspect the schema first; if chain, time window, or candidate count is missing, ask before executing.

## Response normalization

Before rendering, normalize the tool response:

- If it is wrapped as `{"raw_output": "...escaped JSON string..."}`, unescape and parse the inner JSON. Repeat if it is wrapped again.
- If parsing fails, state the parse error. Do not summarize the wrapper as if it were the result.
- Keep numbers, tickers, chain/venue names, status strings, risk flags, timestamps, confidence, skill ids, and `unique_name` verbatim. Narrative text (conclusions, warnings, descriptions) is not copied through in the service's language — it is rendered in the user's language per the Language section below.

## Language

The CMC Crypto Skill Hub services return their text (conclusions, narratives, anomaly and risk descriptions, takeaways) in English. **This is source evidence, not the output language.** Render the final answer in the language of the user's most recent message — translate the English prose into that language rather than passing it through. A Chinese request gets a Chinese answer, a Japanese request a Japanese answer, and so on. If the user mixes languages or writes in English, follow the user.

Keep verbatim, regardless of output language:

- all numbers, percentages, and currency amounts;
- tickers and token names (BTC, ETH, VELVET, …), chain and venue names;
- `status`, `confidence`, `error_code`, timestamps, `skill_id`, `unique_name`;
- the fixed section labels (`**TL;DR**`, `🚨 **Notable anomalies:**`, `📰 **Macro News:**`, `**Details**`, `💡 **Takeaway:**`) — these stay in English in every language.

Everything else — every sentence, bullet description, and Details body that came back in English — is written in the user's language. Do not produce a mostly-English answer for a non-English request.

## Error and status handling

If a tool returns an error, state the exact `error_code` and reason. If the client reports a transport error instead, state it literally. Never fabricate values.

If the user explicitly asked for a CMC Crypto Skill Hub result, stop after the error. For a general crypto question you may continue with allowed external sources, disclosing that the service failed and lowering confidence.

If `status` is `blocked`: skip Details; in TL;DR use the block reason from `conclusion` as the first sentence; suggest one fallback only if the response provides one; end with the footer.

If `status` is `partial`, or `missing_or_stale_inputs` is non-empty, or the response reports inputs that are missing, stale, unavailable, proxied, or low coverage (even when `status` is `ok`): add a `⚠️` callout at the top of TL;DR naming what is missing, render available data, and use lower-confidence language.

## Output Template Router

After a successful run, choose the report template from the executed `unique_name`. This selects formatting only; it does not change which service you ran.

The `unique_name` lists below are **examples, not exhaustive** — services get added and renamed. Match by the service's purpose and the shape of its returned data, not only by an exact name. `find_skill` also returns each candidate's `resultType` and `skill_description`, which help disambiguate.

| Template | Example `unique_name` (match by purpose, not just these) | Read |
|---|---|---|
| Daily overview | `daily_market_overview` (incl. `_debug` / `_v*`), `build_daily_market_brief` | `reference/template-overview.md` |
| Macro read | `crypto_macro_overview`, `macro_liquidity_monitor`, `macro_financial_conditions`, `macro_news_aggregator`, `detect_market_regime`, `decode_macro_event_impact` | `reference/template-macro.md` |
| Scanner / ranking | `onchain_token_scanner`, `altcoin_breakout_scanner_spot`, `altcoin_scanner_perp`, and other `rank_*` / `screen_*` discovery scans | `reference/template-scanner.md` |
| Comparison / cost | `compare_cex_vs_dex_true_cost`, `compare_order_route_true_cost_cex_dex`, `compare_cross_venue_true_cost`, `compare_fee_tier_effective_cost`, `model_fee_tier_trade_cost` (returned `resultType: comparison_pack`) | `reference/template-comparison.md` |
| Attribution / portfolio | `attribute_portfolio_pnl_drivers`, `rank_portfolio_pnl_driver_buckets` (`attribution_pack`); `analyze_portfolio_exposure_map`, `map_portfolio_concentration_buckets`, `portfolio_analysis`, `review_options_portfolio_greeks` | `reference/template-attribution.md` |
| Standard (default) | anything that fits none of the above | Standard Report Format below |

Rules:

1. If the executed service fits a row (by name or purpose) and this client can read bundled files, read that reference and render per its skeleton.
2. If it fits none, the reference cannot be read, or you are unsure, use the Standard Report Format. Falling back is always safe.
3. Field names in the templates are **typical, not guaranteed** — a field may be absent or nested differently (for example `ranked_candidates` at the top level in one service but under `report` in another). Map by meaning; if a field is missing, omit that part rather than inventing it, or fall back to the Standard Report Format.
4. Every template shares this file's rules: TL;DR first, no tables, no raw wrapper, match the user's language, fixed section labels, exact `———` divider, and the footer. A reference only changes how Details is organized.

## Standard Report Format

Plain Markdown for chat clients. No tables. No HTML. Keep normal reports at or below 2500 characters unless the user asks for depth; scanner reports may reach 4000 when candidate lists require it. Match the language of the user's most recent message.

```text
**TL;DR**

Sentence 1 — what the result found.
Sentence 2 — the bottom line and what to do or avoid.
Sentence 3 — the 1-2 key numbers behind it.

🚨 **Notable anomalies:**

- 1-3 bullets, or (none significant in this run)

———

**Details**

**Topic**

- evidence bullets, numbers verbatim

💡 **Takeaway:** what it means, what to watch, a concrete next step.

🕐 timestamp · status · confidence
```

Append `· skill_id` to the footer when present. Use `n/a` for missing footer values; never invent timestamp, status, confidence, or skill id.

Start the answer directly with `**TL;DR**` — no preamble, acknowledgement, or process line (for example "data received, rendering the report…"). The only horizontal divider is the single `———` between TL;DR and Details; never emit `---` or `***` anywhere, including before the report.

Fixed section labels — do not translate: `**TL;DR**`, `🚨 **Notable anomalies:**`, `📰 **Macro News:**`, `**Details**`, `💡 **Takeaway:**`. Include `📰 **Macro News:**` only when the response or allowed external sources carry macro news or key events. For blocked responses omit `———` and Details. Group Details by the response's natural topics; do not force a fixed taxonomy.

## Self-check before answering

Always verify:

- the response was parsed (no `raw_output` string shown as the result);
- no table, no HTML, no raw wrapper, no tool trace;
- the answer starts directly at `**TL;DR**` with no preamble, and contains no `---` or `***` — only the single `———`;
- identifiers keep literal underscores (`price_change_4h`), not backslash-escaped (`price\_change\_4h`);
- the narrative is in the user's language — the English prose from the service was translated, not copied through; only numbers, tickers, IDs, status/confidence strings, and the fixed labels stay verbatim;
- the footer is present with real values or `n/a`.

If you used the Standard Report Format, also verify exactly one `**TL;DR**`, one `———`, and one `**Details**` for non-blocked reports, with `🚨 **Notable anomalies:**` and at least one `💡 **Takeaway:**`. If you used a matched template, verify its own "Minimum" checklist instead.

## Spacing

One blank line between distinct blocks. Exactly one `———` between TL;DR and Details, nowhere else. Bullet marker is `-`; one bullet per line. Write underscores in identifiers and field names literally — do not backslash-escape them (`price_change_4h` and `skill_id`, never `price\_change\_4h`).
