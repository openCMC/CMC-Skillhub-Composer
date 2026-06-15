# Attribution / portfolio report template

Applies to: portfolio decomposition services — PnL attribution (`resultType: attribution_pack`, e.g. `attribute_portfolio_pnl_drivers`, `rank_portfolio_pnl_driver_buckets`) and exposure/concentration maps (`evidence_pack`, e.g. `analyze_portfolio_exposure_map`, `map_portfolio_concentration_buckets`, `portfolio_analysis`, `review_options_portfolio_greeks`). Match by purpose, not just these names.
Render only from the parsed `execute_skill` response. **Field paths vary** — the fields below may sit at the top level or under `report.`; map by meaning, omit anything absent.
Language: the service returns English text; write the narrative (TL;DR, anomaly/bullet text, Details prose, takeaway) in the user's language. Keep numbers, tickers, chain/venue names, status/confidence strings, IDs, and the fixed labels verbatim. The skeleton below is a structure guide, not English output to copy.

## What these services return

`summary` + the decomposition + `decision_basis` + `action_guidance.priority_actions` + `freshness_note`:

- PnL attribution: `total_pnl_usd`, `pnl_window.{start,end}`, `primary_drivers[]{name, pnl_usd}`, `best_bucket`, `worst_bucket`, `attribution_granularity`.
- Exposure map: `total_portfolio_value_usd`, `top_assets[]{name, value_usd}`, `top_chains[]`, `top_venues[]`, `top_strategies[]`, `asset_concentration_pct`, `concentration_bucket`.

## Render skeleton

```text
**TL;DR**

[⚠️ if partial / missing tags: note coverage gaps]
Sentence 1 — the headline (summary): total PnL or total value + concentration.
Sentence 2 — the biggest driver / most concentrated bucket and what it means.
Sentence 3 — the key number(s).

🚨 **Notable anomalies:**

- heavy concentration, one-off gains, large loss bucket, or (none significant in this run)

———

**Details**

**📊 Breakdown**  [pick what the report carries]
- top contributors ranked: NAME — $value (or PnL)
- best bucket / worst bucket, or top asset / chain / venue / strategy

**🎯 Concentration / Window**
- concentration: top asset X% (bucket: high/…); or PnL window start–end

**🧮 Basis**
- one bullet per decision_basis item

💡 **Takeaway:** the main risk/driver and the first thing to act on (from priority_actions). Research context only.

🕐 timestamp · status · confidence · skill_id
```

## Field mapping

| Response field | Renders as |
|---|---|
| `summary` | TL;DR sentence 1 |
| `report.primary_drivers[]` / `top_assets[]` etc. | `📊 Breakdown`, ranked by value/PnL |
| `report.best_bucket` / `worst_bucket` | Breakdown highlights |
| `report.asset_concentration_pct` / `concentration_bucket` / `pnl_window` | `🎯 Concentration / Window` |
| `action_guidance.priority_actions[]` | Takeaway |

## Minimum (loose pass line)

- Headline (total PnL or total value + concentration) in TL;DR.
- Contributors rendered as a **ranked non-table bullet list**.
- Concentration or PnL window surfaced.
- Footer present.

## Bonus

- Best/worst buckets called out; chain/venue/strategy breakdowns; one-off vs repeatable note.

## Never

- Tables; treat modeled values as audited balances; expose raw ledger rows or internal account ids; omit the research-context caveat.
