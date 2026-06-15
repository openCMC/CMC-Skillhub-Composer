# Daily overview report template

Applies to `unique_name`: `daily_market_overview` (incl. `_debug`, `_v*` variants).
Render only from the parsed `execute_skill` response. Field notes below are for mapping.
Language: the service returns English text; write the narrative (TL;DR, anomaly/bullet text, Details prose, takeaway) in the user's language. Keep numbers, tickers, chain/venue names, status/confidence strings, IDs, and the fixed labels verbatim. The skeleton below is a structure guide, not English output to copy.

## What this service returns

Top-down daily crypto brief under `result.data`:

- `status`, `confidence`.
- `decision_report.title`, `.conclusion`, `.market_analysis` — `market_analysis` is a long, already-sectioned body (e.g. Macro Regime, Risk Gates, Liquidity, Cross-Asset, Structure) and is the main content.
- Sometimes present: `market_read`, `macro_deep_read`, `watchlist`, `risk_flags`.
- `missing_or_stale_inputs` — array of input names.
- `action_guidance.bias` (`context_only` / `weak` / `moderate` / `strong`), `.reference_action`.

Never render these internal fields: `signal_board`, `coverage_diagnostics`, `coverage_gap_index`, `data_insights`, `trader_assessment`, `trader_readouts`, `next_research_actions`, `output_budget`.

## Render skeleton

```text
**TL;DR**

[⚠️ if status=partial or missing_or_stale_inputs non-empty: one line naming the missing inputs]
Sentence 1 — the top-down read (condense decision_report.conclusion).
Sentence 2 — bottom line: what to lean toward or avoid.
Sentence 3 — 1-2 key numbers.

🚨 **Notable anomalies:**

- 1-3 bullets, or (none significant in this run)

———

**Details**

[Render decision_report.market_analysis, keeping its existing sections — one bold header per section with its bullets, numbers verbatim.]

[If watchlist present:]
**👀 Watchlist**
- SYMBOL — one-line reason

[If risk_flags present:]
**⚠️ Risk**
- flag, quoted literally

💡 **Takeaway:** from action_guidance.reference_action; tone must not exceed action_guidance.bias.

🕐 timestamp · status · confidence · skill_id
```

## Field mapping

| Response field | Renders as |
|---|---|
| `decision_report.conclusion` | TL;DR sentences 1-2 |
| `decision_report.market_analysis` | Details body, sections preserved |
| `missing_or_stale_inputs` | top `⚠️` callout when non-empty |
| `watchlist` / `risk_flags` | their own blocks (non-table) |
| `action_guidance.reference_action` | Takeaway |
| `action_guidance.bias` | caps the tone of the whole report |
| `status` / `confidence` / `skill_id` / `timestamp` | footer |

## Minimum (loose pass line)

- TL;DR is conclusion-first.
- Details preserves the `market_analysis` sections, not flattened into one blob.
- Missing inputs surfaced when partial.
- Footer carries real status and confidence.

## Bonus

- `watchlist` and `risk_flags` as their own blocks; anomalies populated with concrete deviations.

## Never

- Render any internal field listed above; use tables; expose provider/SDK names; give buy/sell advice when `bias` is `context_only`.
