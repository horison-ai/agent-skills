---
name: precedent-lookup
description: Search the firm's institutional memory in Horison for prior experience with a topic, pattern, company, or risk — past claims, decisions, and the deals where they arose. Use when asking "have we seen this before", checking precedent before an IC, or recovering why the firm passed on something. Triggers on "have we seen this", "precedent", "prior experience with", "what did we decide on", or "institutional memory". Requires the Horison MCP connector.
---

# Precedent Lookup

Answers from the firm's recorded experience — claims people made,
decisions taken, and the deals they arose in — never from general
knowledge. If the Horison tools are not available, stop and tell the user
to connect the Horison MCP server first (Horison → Settings → Integrations
→ Connect your agent).

## Workflow

### Step 1: The firm's own record

1. `get_firm_memory(query=<topic>)` — claims and decisions on the topic:
   who asserted what, with what stance (for/against/neutral), and what was
   decided. This is the highest-authority source; lead with it.
2. `search_firm_knowledge(query=<topic>)` — portfolio and precedent
   companies, sector theses, firm context matching the topic.

### Step 2: The evidence across deals

3. `search_knowledge_graph(query=<topic>)` — tenant-wide, no `deal_id`, so
   it spans every deal the user can see.
4. For each concrete company/person/metric that surfaces:
   `compare_entity_across_deals(entity_name=...)` — where else the firm has
   encountered it and in what role.
5. `find_evidence(query=<topic>)` — the strongest cited passages backing
   the pattern.

### Step 3: Synthesize

Build the precedent record:

| Deal | When | What happened | Firm's claim/decision | Outcome / open |
|---|---|---|---|---|

Then answer directly: has the firm seen this pattern before, in which
deals (deep links), what did it conclude then, and which of those
questions apply to the current situation?

## Important Notes

- Distinguish the three evidence classes and label them: **decision**
  (the firm chose), **claim** (someone asserted, with a stance), and
  **document evidence** (a source said). A past claim is not a past
  decision.
- If the graph has no precedent, say so explicitly — "no recorded
  precedent" is a real and useful answer; do not pad it with general
  industry knowledge unless the user asks, and then label it as outside
  the firm's record.
- Precedent is permission-scoped: deals the user cannot see do not appear.
  Note this when the user is asking on the whole firm's behalf.
- Carry stances faithfully — if two partners disagreed in the record,
  show both sides rather than the average.
