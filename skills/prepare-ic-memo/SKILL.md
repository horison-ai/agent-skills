---
name: prepare-ic-memo
description: Prepare an Investment Committee memo for a deal from the Horison deal-intelligence graph. Use when the user asks to draft an IC memo, investment memo, deal write-up, or committee paper for a deal. Requires the Horison MCP connector (read-only deal knowledge graph, documents, and firm memory).
---

# Prepare an IC memo

You are preparing an Investment Committee memo for the deal the user names.
Work through the Horison tools in order, citing the deep links each returns.
If the Horison tools are not available, stop and tell the user to connect the
Horison MCP server first (Horison → Settings → Integrations → Connect your
agent).

1. If the deal reference is not already a `deal_id` (UUID), call
   `list_deals(search=<name>)` to resolve it.
2. `get_deal_overview(deal_id)` — document inventory and entity census; note
   coverage gaps.
3. `get_metric_history(deal_id=..., include_superseded=true)` — headline
   financials (revenue, EBITDA, margins) and how they changed across document
   versions; flag any figure that was revised.
4. `search_themes(query="risks and red flags", deal_id=...)` — the deal's
   aggregated risk/theme panels; follow up with
   `find_evidence(query="key risks", deal_id=...)` for cited passages on
   anything material.
5. `get_firm_memory(kind="criterion")` — the firm's investment criteria; map
   the deal against them. Add `search_firm_knowledge(query=<deal's sector>)`
   for precedent companies and theses.
6. `get_document_insights(deal_id=...)` — per-document summaries for anything
   thin.

Then write the memo skeleton: Executive Summary, Business Overview, Financial
Performance (with the version-discrepancy flags from step 3), Key Risks, Fit
vs Firm Criteria, Precedents & Firm Context, Open Questions.

Every factual claim must carry its source deep link from the tool results.
Do not invent figures — if a number is not in the tool output, list it under
Open Questions.
