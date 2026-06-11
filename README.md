# Horison Agent Skills

PE deal-work methodologies for AI agents connected to the
[Horison](https://horison.ai) MCP server — IC memo preparation, DD gap
analysis, deal comparison, pipeline review, precedent lookup, and metric
deep dives. Each skill names the exact Horison tool-call sequence and the
output format, so any MCP-capable agent executes the firm's workflow the
same way.

The skills are plain Agent Skills (`skills/<name>/SKILL.md`) with no
client-specific syntax — the same files work in Claude Code, Claude
Cowork, and claude.ai.

## Prerequisite — connect your agent to Horison

The skills drive Horison's read-only MCP tools (deal knowledge graph,
documents, metrics, firm memory). Connect first: in Horison go to
**Settings → Integrations → Connect your agent** and follow the steps for
your client. Authentication is OAuth through your Horison account; every
skill sees only the deals and documents your account can see.

## Use the skills

**Claude Code / Cursor / VS Code**

```bash
npx skills add horison-ai/agent-skills
```

**claude.ai / Claude Cowork** — upload the skill folders under
Settings → Capabilities → Skills (each folder under `skills/` is one
skill).

**ChatGPT** — ChatGPT has no skills system; the same methodologies are
built into the Horison MCP server as prompts, so once the connector is
added the workflows are available without installing anything.

## Skills

| Skill | Use it for |
|---|---|
| `prepare-ic-memo` | Investment Committee memo skeleton with cited evidence |
| `dd-gap-analysis` | Where the data room is covered / thin / missing |
| `compare-deals` | Two deals side by side, shared entities included |
| `pipeline-review` | Stage, diligence depth, and attention flags across live deals |
| `precedent-lookup` | "Have we seen this before?" across deals and firm memory |
| `metric-deep-dive` | One metric's full version history and discrepancy flags |

The skill bodies mirror the MCP prompts served by the Horison server
(`prepare_ic_memo`, `dd_gap_analysis`, …) — if a prompt's methodology
changes, change the matching skill in the same release.
