---
name: prepare-ic-memo
description: Draft a structured Investment Committee memo for a deal, grounded in the firm's Horison deal-intelligence graph — every figure and risk cited to its source document. Use when preparing for investment committee, writing up a deal, or creating a formal recommendation. Triggers on "write IC memo", "investment committee memo", "deal write-up", "prepare IC materials", or "recommendation memo". Requires the Horison MCP connector.
---

# Investment Committee Memo

Drafts an IC-ready memo where the evidence comes from the firm's own data
room and knowledge graph via Horison, not from memory. If the Horison tools
are not available, stop and tell the user to connect the Horison MCP server
first (Horison → Settings → Integrations → Connect your agent).

## Workflow

### Step 1: Resolve the deal and take inventory

1. If the deal reference is not already a `deal_id` (UUID), call
   `list_deals(search=<name>)` to resolve it.
2. `get_deal_overview(deal_id)` — document inventory and entity census.
   Note what the data room does NOT contain; those become Open Questions,
   not guesses.

### Step 2: Pull the evidence base

3. `get_metric_history(deal_id=..., include_superseded=true)` — headline
   financials (revenue, EBITDA, margins) and how they changed across
   document versions. Flag every figure that was revised between documents;
   IC members will ask why.
4. `search_themes(query="risks and red flags", deal_id=...)` — the deal's
   aggregated risk/theme panels. For anything material, follow up with
   `find_evidence(query=<the specific risk>, deal_id=...)` to get cited
   passages.
5. `get_firm_memory(kind="criterion")` — the firm's investment criteria.
   `search_firm_knowledge(query=<deal's sector>)` — precedent companies and
   theses the firm already holds.
6. `get_document_insights(deal_id=...)` — per-document summaries for any
   section still thin.

### Step 3: Gather what Horison cannot know

Ask the user (or take from earlier in the session) for inputs that live
outside the data room: proposed deal terms (price, structure, financing),
returns analysis (base / upside / downside, IRR and MOIC), value-creation /
100-day plan, and the management assessment. Never invent these.

### Step 4: Draft the memo

Standard structure:

**I. Executive Summary** (1 page) — company description, deal rationale,
key terms, recommendation, headline returns, top 3 risks with mitigants.

**II. Business Overview** — products/services, customers, go-to-market,
competitive positioning, management. Source from steps 2 and 6.

**III. Industry & Market** — market size/growth, competitive landscape,
trends. Use `search_knowledge_graph(query="market", deal_id=...)` plus the
firm's sector theses from step 5.

**IV. Financial Performance** — historicals from step 3's metric history,
as tables not prose. Include the version-discrepancy flags explicitly
("CIM states €Xm; QoE adjusts to €Ym — link both").

**V. Investment Thesis & Fit vs Firm Criteria** — 3–5 pillars, each mapped
against the firm criteria from step 5; say plainly where the deal misses a
criterion.

**VI. Deal Terms & Structure** — from the user's step-3 inputs.

**VII. Returns Analysis** — from the user's step-3 inputs; note which
assumptions Horison evidence supports or contradicts.

**VIII. Key Risks** — ranked by severity, each with its evidence deep link
and a mitigant. Include risks where evidence is absent ("no churn data in
the data room") — absence of evidence is a risk.

**IX. Recommendation & Open Questions** — Proceed / Pass / Conditional,
plus everything from steps 1–3 that the data room could not answer.

## Important Notes

- Every factual claim carries its Horison deep link. A number without a
  source goes under Open Questions, never in a financial table.
- Be balanced — present the bear case honestly; IC members find omitted
  risks anyway and credibility is the asset.
- Financial tables must tie: if the EBITDA bridge from metric history
  doesn't reconcile across documents, show the discrepancy rather than
  silently picking one figure.
- Use the firm's memo template if the user provides one; otherwise the
  structure above.
- Output: Markdown by default; offer a .docx export if the client supports
  document generation.
