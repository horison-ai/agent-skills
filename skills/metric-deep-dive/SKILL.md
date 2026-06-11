---
name: metric-deep-dive
description: Trace one financial metric for a deal through every document version in Horison — full history, source passages, and explicit discrepancy flags where documents disagree. Use when checking where a number came from, why figures differ between the CIM and the QoE, or before quoting a number to IC. Triggers on "where does this number come from", "metric history", "why do the figures differ", "verify EBITDA", or "deep dive on [metric]". Triggers also when two documents state different values for the same period. Requires the Horison MCP connector.
---

# Metric Deep Dive

Reconstructs a metric's full life across the data room — every recorded
value, who said it, and where documents disagree. If the Horison tools are
not available, stop and tell the user to connect the Horison MCP server
first (Horison → Settings → Integrations → Connect your agent).

## Workflow

### Step 1: The full history

1. Resolve the deal with `list_deals` if needed.
2. `get_metric_history(entity_name=<metric>, deal_id=...,
   include_superseded=true)` — every recorded state: value, unit, currency,
   validity window, recording document, and superseded versions. The
   superseded chain is the point — do not drop it.

### Step 2: The sources

3. `find_evidence(query=<metric>, deal_id=..., scope="metrics")` — cited
   source passages and time-series context for the values.
4. Where a cited table looks fragmented or a passage is ambiguous,
   `read_document(file_id, ...)` around the cited section —
   `read_document` returns clean, unfragmented text and is the ground
   truth when a summary and a source disagree.

### Step 3: Reconcile

For every period where two documents state different values, build a
discrepancy entry: both values, both deep links, and the explanation if
the sources give one (restatement, adjustment, scope change, currency,
definition — e.g. reported vs adjusted EBITDA). If no source explains the
difference, say "unreconciled" — that is a diligence finding, not a
formatting problem.

## Output

1. **Current value** with unit, currency, period, and source deep link.
2. **Version history table**: value | unit/currency | period | document |
   recorded | superseded? — one row per recorded state.
3. **Discrepancy flags**: period | value A (link) | value B (link) |
   severity | explanation or "unreconciled".
4. **Assessment**: how much confidence the number deserves in an IC memo,
   given its history (stable across documents / revised once with
   explanation / unreconciled).

Severity scale for discrepancies: **definitional** (different metric
flavors, explained) · **revision** (later document supersedes, explained)
· **conflict** (same period, same definition, different numbers,
unexplained — escalate).

## Important Notes

- Numbers must carry unit, currency, and period everywhere — most "
  discrepancies" in PE data rooms are definitional (LTM vs FY, reported vs
  adjusted, EUR vs GBP); check definitions before declaring a conflict.
- Quote the source's own words for any adjustment narrative rather than
  paraphrasing — adjustments are exactly where paraphrase introduces
  errors.
- Read the source document (Step 2.4) before relying on a summary;
  summaries round and omit line items.
- If the metric simply isn't in the graph, say so and suggest the nearest
  recorded metrics by name — do not substitute a "similar" number
  silently.
