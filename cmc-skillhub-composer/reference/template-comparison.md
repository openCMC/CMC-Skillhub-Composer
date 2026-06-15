# Comparison / cost report template

Applies to: trading-cost comparison services (`resultType: comparison_pack`) — e.g. `compare_cex_vs_dex_true_cost`, `compare_order_route_true_cost_cex_dex`, `compare_cross_venue_true_cost`, `compare_fee_tier_effective_cost`, `model_fee_tier_trade_cost`. Match by purpose, not just these names.
Render only from the parsed `execute_skill` response.
Language: the service returns English text; write the narrative (TL;DR, anomaly/bullet text, Details prose, takeaway) in the user's language. Keep numbers, tickers, chain/venue names, status/confidence strings, IDs, and the fixed labels verbatim. The skeleton below is a structure guide, not English output to copy.

## What these services return

A ranked cost comparison under `result.data`. **Field paths vary by service** — the ranked list and recommendation may sit at the top level (`ranked_candidates`, `recommended_route`) in one service and under `report.` in another (fee-tier services use `ranked_venues` / `recommended_venue`). Map by meaning, not by an exact path.

- `status`, `summary` — one-sentence verdict (e.g. cheapest route).
- `mode`, `order_size`, `urgency` (or `expected_monthly_volume_usd`, `trade_profile` for fee-tier services) — at top level or under `report`.
- ranked list (`ranked_candidates` / `ranked_venues`) — each row typically: `venue`, `venue_type`, `fee_bps`, `slippage_bps`, `fixed_cost_usd`, `latency_score`, `modeled_total_cost_usd` (fee-tier rows differ).
- recommendation (`recommended_route` / `recommended_venue`) — the winning row.
- `decision_basis[]`, `action_guidance.priority_actions[]`, `freshness_note`.
- A `blocked`/`error` status usually means required caller inputs were missing — surface that honestly rather than inventing a ranking.

## Render skeleton

```text
**TL;DR**

[⚠️ if status=partial: note what is modeled vs missing]
Sentence 1 — the recommended route and why (summary).
Sentence 2 — order size / urgency context and the cost gap vs the runner-up.
Sentence 3 — the key cost numbers (total cost, fee/slippage bps).

🚨 **Notable anomalies:**

- large cost gaps, high slippage, latency penalties, or (none significant in this run)

———

**Details**

**🏆 Recommended**
- VENUE (type) — total ~$X; fee Ybps, slippage Zbps, fixed $F, latency L

**⚖️ Ranked Routes**
- VENUE (type) — total ~$X; fee Ybps, slippage Zbps  [one bullet per candidate, cheapest first]

**🧮 Basis**
- one bullet per decision_basis item

💡 **Takeaway:** which route to use and what to check first (from priority_actions). Research context only.

🕐 timestamp · status · confidence · skill_id
```

## Field mapping

| Response field | Renders as |
|---|---|
| `summary` + `report.recommended_route` | TL;DR + `🏆 Recommended` |
| `report.ranked_candidates[]` | `⚖️ Ranked Routes` bullets (cheapest first) |
| `report.order_size` / `report.urgency` | TL;DR context |
| `decision_basis[]` | `🧮 Basis` bullets |
| `action_guidance.priority_actions[]` | Takeaway |

## Minimum (loose pass line)

- Recommended route stated with its total modeled cost.
- Candidates rendered as a **non-table bullet list**, cheapest first.
- Order size / urgency context surfaced.
- Footer present.

## Bonus

- Per-route fee/slippage/latency breakdown; explicit cost gap between top two.

## Never

- Tables; treat modeled cost as guaranteed execution cost; expose venue/provider internal names beyond what the response surfaces; omit the "modeled / research context" caveat.
