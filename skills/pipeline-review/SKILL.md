---
name: pipeline-review
description: Produce a partner-ready review of the live deal pipeline from Horison — stage, diligence depth, momentum, and attention flags per deal. Use for weekly pipeline meetings, partner updates, or a quick "what's moving" check. Triggers on "pipeline review", "deal pipeline", "what's moving", "weekly update", or "which deals need attention". Requires the Horison MCP connector.
---

# Pipeline Review

A status review built only from evidenced platform data — what each deal's
data room and graph actually show, never inferred activity. If the Horison
tools are not available, stop and tell the user to connect the Horison MCP
server first (Horison → Settings → Integrations → Connect your agent).

## Workflow

### Step 1: The pipeline

`list_deals()` — every deal visible to the user, with stage and status.
Partition into active vs archived/closed; the review covers active deals
only unless the user asks otherwise.

### Step 2: Diligence depth per deal

For each active deal: `get_deal_overview(deal_id)` — document count and
entity census. Treat these as the proxy for diligence progress: documents
ingested, entities and metrics extracted, themes formed.

### Step 3: Headline numbers for the front of the pipeline

For the 2–3 most advanced deals:
`get_metric_history(deal_id=..., entity_name="EBITDA")` — headline
financials with their as-of documents, so the partner meeting has numbers
with sources.

### Step 4: Flag

Assign each deal a flag:
- **Green** — data room growing, evidence consistent with its stage
- **Yellow** — stage says diligence but the data room is thin (few
  documents, sparse entity census) — knowledge is behind the process
- **Red** — stalled: documents present but nothing new evidenced, or
  material risk themes unaddressed in the question backlog

## Output

1. Pipeline table: deal | stage | status | docs | diligence depth | flag |
   deep link.
2. Short narrative — what moved, what is stalled, which 2–3 deals need
   attention this week and the specific reason (e.g. "stage = DD but no
   QoE in the data room").
3. Headline numbers for the front-of-pipeline deals, each with its source
   document link.

## Important Notes

- Use only tool data. "No new documents" is a fact; "the team has gone
  quiet" is an inference — report the former, not the latter.
- Document count is a proxy, not truth: a deal can be active in meetings
  while the data room lags. Phrase yellow flags as "the data room is
  behind", not "the deal is behind".
- The pipeline is permission-scoped: this is the connected user's view,
  not necessarily the whole firm's — say so in the header if presenting
  to others.
- Keep it board-ready: concise, factual, no filler.
