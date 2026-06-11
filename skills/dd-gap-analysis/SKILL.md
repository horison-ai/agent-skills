---
name: dd-gap-analysis
description: Audit due-diligence coverage for a deal against the evidence actually in the Horison data room — per workstream, with covered/thin/missing assessments and red flags. Use when kicking off diligence, reviewing data-room completeness, or deciding what to request next. Triggers on "dd gaps", "what do we still need", "data room review", "diligence coverage", or "dd checklist". Requires the Horison MCP connector.
---

# Due-Diligence Gap Analysis

Tests each diligence workstream against the evidence that actually exists
in the deal's Horison graph — coverage is measured, not assumed. If the
Horison tools are not available, stop and tell the user to connect the
Horison MCP server first (Horison → Settings → Integrations → Connect your
agent).

## Workflow

### Step 1: Scope

1. Resolve the deal with `list_deals(search=<name>)` if needed.
2. `get_deal_overview(deal_id)` — the document inventory and entity census.
   Note the deal's sector; it drives Step 2's additions.
3. Ask the user for deal type (platform / add-on / growth / carve-out) and
   any known concerns to prioritize, if not obvious from the overview.

### Step 2: Probe each workstream

For every workstream below, call
`search_knowledge_graph(query=<workstream terms>, deal_id=...)` and judge
the evidence that returns. Standard workstreams:

- **Financial** — quality of earnings, revenue/EBITDA adjustments, working
  capital, debt and debt-like items, capex (maintenance vs growth), tax,
  audit history
- **Commercial** — market size/growth, competitive position, customer
  concentration and retention, pricing power, pipeline/backlog
- **Legal** — corporate structure, material contracts, litigation, IP,
  regulatory compliance, employment agreements
- **Operational** — management team, key-person risk, IT systems, supply
  chain and vendor dependencies, facilities, insurance
- **People** — headcount trends, compensation, retention risk, pensions,
  labor agreements
- **Sector-specific additions** — Software/SaaS: ARR quality, cohorts,
  hosting costs, SOC2 · Healthcare: regulatory approvals, reimbursement,
  payor mix · Industrial: equipment condition, environmental, safety ·
  Consumer: brand health, channel mix, seasonality

### Step 3: Confirm the worst gaps are real

`find_evidence(query=<area>, deal_id=...)` on the two thinnest areas — a
gap can be a naming mismatch rather than missing evidence. Only call it
"missing" after the evidence search also comes back empty.

### Step 4: Fold in the question backlog

`get_firm_memory(kind="question", deal_id=...)` — the firm's own open
questions on the deal; `get_suggested_questions(deal_id=...)` — the
AI-generated per-document ones. Gaps that already have an open question
are "known"; gaps without one are new findings.

## Output

A coverage table:

| DD area | Evidence found (deep links) | Assessment | Follow-up |
|---|---|---|---|
| Financial — QoE | QoE report §3, CIM p.12 | Covered | — |
| Commercial — customers | 2 mentions, no churn data | Thin | Request cohort file |
| Legal — litigation | none | **Missing** | Add to request list |

Assessment scale: **Covered** (dedicated document + corroborating
evidence) · **Thin** (mentions only, no dedicated analysis) · **Missing**
(nothing returns). Close with: the red-flag list (gap, severity —
deal-breaker / significant / manageable — and the request that resolves
it), and which workstreams lack a dedicated DD report entirely.

## Important Notes

- Coverage ≠ document count: a 200-page CIM can still leave QoE missing.
  Judge by workstream evidence, not volume.
- A slow-to-fill gap that the seller controls is itself a signal — note
  persistent gaps across re-runs.
- Deep-link every "covered" claim; an unsourced "covered" is worthless in
  the request-list discussion.
- Results are scoped to the connected user's permissions — if the user
  expects a document that doesn't appear, access is one possible reason;
  say so rather than declaring it absent.
