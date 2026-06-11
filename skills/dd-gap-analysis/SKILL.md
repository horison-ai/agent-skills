---
name: dd-gap-analysis
description: Run a due-diligence gap analysis for a deal against the evidence in the Horison knowledge graph. Use when the user asks what diligence is missing, where the DD gaps are, or how complete the data room coverage is. Requires the Horison MCP connector.
---

# Due-diligence gap analysis

You are running a due-diligence gap analysis for the deal the user names. If
the Horison tools are not available, stop and tell the user to connect the
Horison MCP server first (Horison → Settings → Integrations → Connect your
agent).

1. If the deal reference is not already a `deal_id` (UUID), resolve it with
   `list_deals(search=<name>)`.
2. `get_deal_overview(deal_id)` — what documents and entity types exist.
3. For each expected DD area — financial (QoE, working capital, debt),
   commercial (market, competition, customers), legal (contracts, litigation,
   IP), operational (technology, supply chain), people (management,
   retention), tax — call `search_knowledge_graph(query=<area>, deal_id=...)`
   and note how much evidence comes back.
4. `find_evidence(query=<area>, deal_id=...)` on the two thinnest areas to
   confirm the gap is real rather than a naming mismatch.
5. `get_firm_memory(kind="question", deal_id=...)` — the firm's open
   questions on the deal; add `get_suggested_questions(deal_id=...)` for the
   AI-generated ones per document.

Produce a table: DD area | evidence found (with deep links) | assessment
(covered / thin / missing) | follow-up questions. Flag areas with no
dedicated DD report in the document inventory.
