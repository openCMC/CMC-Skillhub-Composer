# Scanner / ranking report template

Applies to: discovery scans / ranked candidate queues — e.g. `onchain_token_scanner`, `altcoin_breakout_scanner_spot`, `altcoin_scanner_perp`, and other `rank_*` / `screen_*` scans. Match by purpose, not just these names.
Render only from the parsed `execute_skill` response. Field paths and names vary by service; map by meaning and omit anything absent.
Language: the service returns English text; write the narrative (TL;DR, anomaly/bullet text, Details prose, takeaway) in the user's language. Keep numbers, tickers, chain/venue names, status/confidence strings, IDs, and the fixed labels verbatim. The skeleton below is a structure guide, not English output to copy.

## What these services return

A candidate list (and sometimes signal rows), not a narrative report:

- a list of scored candidates — typical fields per row: `symbol`, `name`, `contract_address`, `network`, `score`, `market_cap_usd`, `liquidity_usd`, `volume_24h`, `price_change_24h`, `price_change_4h`, and any `missing_*` fields. (In some services these sit under `runtime_summary.raw_candidates[]`.)
- optionally, signal rows the service already filtered (e.g. `runtime_summary.raw_signals_sample[]`). The service applies its own thresholds (such as max-gain, min market cap, max age); **state the thresholds it reports — do not re-filter or recompute them yourself.**
- coverage info — processed / rejected / passed counts and any missing indicators.

## Render skeleton

```text
**TL;DR**

[⚠️ if no signal row passed: say so and keep partial]
Sentence 1 — how many candidates surfaced and how many signals passed.
Sentence 2 — the standout name(s) and why.
Sentence 3 — the key filter thresholds applied.

🚨 **Notable anomalies:**

- concentration, large 24h move, unknown security, etc.

———

**Details**

**🥇 Top Candidates**

- SYMBOL (network) — liquidity $X, score Y [, 24h ±Z%]; note missing fields if any
- ... (cap the list; if long, show top N and state how many more)

**🚦 Signals** (rows passing all filters)

- SYMBOL (network) — max gain X%, mcap $Y, age Zh, 24h ±W%, security: <level>
- [if none passed: "No live signal rows passed the filters this run."]

**⚠️ Coverage**

- processed N: rejected a (gain), b (mcap), c (age); passed d
- missing indicators: ... (if any)

💡 **Takeaway:** the strongest candidate/signal and what to watch. Research context only — not an execution instruction.

🕐 timestamp · status · confidence · skill_id
```

## Minimum (loose pass line)

- Candidates rendered as a **non-table bullet list** (this is the fix for the observed table-render failures).
- Filter thresholds stated.
- The "no signal passed" case handled explicitly.
- Footer present.

## Bonus

- Per-candidate `missing_scoring_fields`; wallet activity / ROI for the top signal; full coverage breakdown.

## Never

- **Markdown tables** — chat clients render them broken; always use bullets.
- Dump `raw_candidates`, `selected_items`, `narrative_context`, or any `runtime_summary` object as JSON or dict text.
- Expose provider names, source-platform names, links, or accounts.
- Narrate anything beyond the top candidate and the top filtered signal row.
