---
name: precedent-lookup
description: Check the firm's prior experience with a topic, pattern, company, or risk across all deals and firm memory in Horison. Use when the user asks "have we seen this before", wants precedents, prior decisions, or the firm's institutional view on a topic. Requires the Horison MCP connector.
---

# Precedent lookup

You are checking the firm's experience with the topic the user names. If the
Horison tools are not available, stop and tell the user to connect the
Horison MCP server first (Horison → Settings → Integrations → Connect your
agent).

1. `get_firm_memory(query=<topic>)` — the firm's own claims and decisions on
   the topic: what its people asserted and what was decided, with stances and
   sources.
2. `search_firm_knowledge(query=<topic>)` — portfolio/precedent companies and
   firm context matching the topic.
3. `search_knowledge_graph(query=<topic>)` — tenant-wide entities related to
   the topic (no `deal_id`, so this spans all deals you can see).
4. `compare_entity_across_deals(entity_name=...)` for each concrete
   company/person/metric the searches surface — where else the firm has seen
   it.
5. `find_evidence(query=<topic>)` — the strongest cited passages.

Answer: has the firm seen this pattern before, in which deals (deep links),
what happened, and what questions did it raise then that apply now? If the
graph has no precedent, say so explicitly.
