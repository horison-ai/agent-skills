---
name: compare-deals
description: Compare two deals side by side from the Horison deal-intelligence graph — financials with sources, risk themes, shared entities, and fit against the firm's criteria. Use when benchmarking two opportunities, allocating partner time, or deciding which deal to advance. Triggers on "compare deals", "X vs Y", "which deal is better", "benchmark against", or "side by side". Requires the Horison MCP connector.
---

# Compare Two Deals

Side-by-side comparison where every cell is sourced from the firm's own
deal evidence. If the Horison tools are not available, stop and tell the
user to connect the Horison MCP server first (Horison → Settings →
Integrations → Connect your agent).

## Workflow

### Step 1: Resolve and inventory both deals

1. Resolve both names with `list_deals` if needed.
2. For EACH deal: `get_deal_overview(deal_id)` — stage, document inventory,
   entity census. Note asymmetry in data-room depth up front: a comparison
   between a 60-document deal and a 6-document deal is a comparison of
   knowledge, not of quality.

### Step 2: Pull comparable evidence

For EACH deal:
- `get_metric_history(deal_id=..., entity_name="EBITDA")` and
  `get_metric_history(deal_id=..., entity_name="revenue")` — record value,
  unit, currency, and the as-of document for each
- `search_themes(query="key risks", deal_id=...)` — the deal's risk panels

### Step 3: Shared-entity analysis

`compare_entity_across_deals(entity_name=...)` for any customer, supplier,
sponsor, or advisor appearing in both deals — shared entities are where
one deal's diligence informs the other (a customer's churn in deal A is
evidence about deal B's pipeline).

### Step 4: Fit against the firm's lens

`get_firm_memory(kind="criterion")` for the firm's investment criteria and
`search_firm_knowledge(query=<sector>)` for precedent context. Score both
deals against the same criteria.

## Output

| Dimension | Deal A | Deal B |
|---|---|---|
| Stage & status | | |
| Revenue (as-of doc, currency) | | |
| EBITDA (as-of doc, currency) | | |
| Data-room depth (docs / entities) | | |
| Top risk themes | | |
| Shared entities | | |
| Fit vs firm criteria | | |

Cite the deep link in every evidence cell. Close with:
1. Which deal screens better on the firm's usual criteria — and on which
   criteria the answer flips.
2. The top 3 open questions for each deal.
3. Any finding from one deal that changes the read on the other (via
   shared entities).

## Important Notes

- Never compare numbers without units, currency, and period — "EBITDA 12
  vs 9" is meaningless if one is LTM EUR and the other FY23 GBP. The
  metric history carries all three; surface them.
- If a metric was revised across documents within one deal, use the most
  recent value but flag the revision — it affects confidence, which is a
  comparison dimension.
- Thin data is a finding, not a tie-breaker by default: say "B looks
  worse but is 80% less documented" rather than scoring B down silently.
- Both deals must be visible to the connected user; an empty side usually
  means a permissions boundary, not an empty deal.
