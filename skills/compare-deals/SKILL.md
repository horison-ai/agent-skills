---
name: compare-deals
description: Compare two deals side by side using the Horison deal-intelligence graph — size, financials, risk themes, and shared entities. Use when the user asks to compare deals, benchmark one deal against another, or decide which of two opportunities screens better. Requires the Horison MCP connector.
---

# Compare two deals

You are comparing the two deals the user names, side by side. If the Horison
tools are not available, stop and tell the user to connect the Horison MCP
server first (Horison → Settings → Integrations → Connect your agent).

1. Resolve both names to `deal_id`s with `list_deals` if needed.
2. For EACH deal run: `get_deal_overview(deal_id)`,
   `get_metric_history(deal_id=..., entity_name="EBITDA")`,
   `get_metric_history(deal_id=..., entity_name="revenue")`, and
   `search_themes(query="key risks", deal_id=...)`.
3. `compare_entity_across_deals(entity_name=...)` on any customer, supplier,
   or sponsor that shows up in both.

Produce a side-by-side table: size & stage | headline financials (with units
and the as-of document) | risk themes | shared entities. Cite the deep links
per cell. Close with which deal screens better on the firm's usual criteria
(`search_firm_knowledge` for context) and the top 3 open questions for each.
