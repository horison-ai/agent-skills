---
name: pipeline-review
description: Prepare a deal-pipeline review from the Horison platform — every active deal, its stage, diligence depth, and what needs attention. Use when the user asks for a pipeline review, portfolio of live deals, weekly deal update, or "what's moving". Requires the Horison MCP connector.
---

# Pipeline review

You are preparing the deal-pipeline review for the firm. If the Horison
tools are not available, stop and tell the user to connect the Horison MCP
server first (Horison → Settings → Integrations → Connect your agent).

1. `list_deals()` — the full pipeline with stage and status.
2. For each active deal (skip archived/closed): `get_deal_overview(deal_id)`
   — document count and entity census as a proxy for diligence progress.
3. For the 2–3 most advanced deals,
   `get_metric_history(deal_id=..., entity_name="EBITDA")` for headline
   numbers.

Produce: a pipeline table (deal | stage | status | docs | diligence depth |
deep link), then a short narrative — what moved, what is stalled (documents
but no recent activity), and which deals need attention. Use only tool data;
do not infer activity that is not evidenced.
