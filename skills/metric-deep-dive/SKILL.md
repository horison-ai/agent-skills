---
name: metric-deep-dive
description: Deep dive on one financial metric for a deal in Horison — full version history, source passages, and version-discrepancy flags across documents. Use when the user asks where a number came from, why figures differ between documents, or for the history of a metric like EBITDA or revenue. Requires the Horison MCP connector.
---

# Metric deep dive

You are doing a deep dive on the metric the user names, for the deal they
name. If the Horison tools are not available, stop and tell the user to
connect the Horison MCP server first (Horison → Settings → Integrations →
Connect your agent).

1. Resolve the deal to a `deal_id` with `list_deals` if needed.
2. `get_metric_history(entity_name=<metric>, deal_id=...,
   include_superseded=true)` — every recorded state: value, unit, currency,
   validity window, and superseded versions.
3. `find_evidence(query=<metric>, deal_id=..., scope="metrics")` — the cited
   source passages and the full time-series context.
4. For each document the evidence cites, `read_document(file_id, ...)` around
   the cited section if the table looks fragmented — `read_document` returns
   clean unfragmented text.

Report: the current value with its source; the full version history as a
table (value | document | recorded | superseded?); explicit
version-discrepancy flags wherever two documents state different values for
the same period (show both deep links); and what reconciles or explains each
discrepancy if the sources say.
