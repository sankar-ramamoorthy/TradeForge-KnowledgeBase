---
title: M14C Research Cockpit Import Bridge
type: brainstorming-note
status: processed-draft
created: 2026-05-27
updated: 2026-05-27
tags:
  - TradeForge
  - M14C
  - Research-Cockpit
  - Import-Bridge
  - advisory-artifact
  - lifecycle-integrity
  - replayability
source:
  - "knowledge/raw/M14C — Research Cockpit Import Bridge.md"
related_milestones:
  - M14C
related_issues:
  - TF-A016
  - TF-A017
  - TF-A018
  - TF-A019
  - TF-A020
  - TF-R001
related:
  - [[Advisory Artifact]]
  - [[Decision Lifecycle Engine]]
  - [[Event Ledger]]
  - [[Replay System]]
---

# M14C Research Cockpit Import Bridge

## Summary

This note proposes a controlled Import Bridge between external research environments and TradeForge lifecycle workflows.

The central idea is that operator-authored or AI-assisted research from a Research Cockpit, ChatGPT, Claude, Codex, or local Markdown workflow can be ingested into TradeForge as draft lifecycle material without becoming canonical automatically.

The imported file is not a thesis. It remains a draft thesis candidate or equivalent advisory/import artifact until the operator explicitly reviews and promotes it.

## Runtime Alignment

Runtime docs already show M12 completed the advisory artifact foundation for this direction:

- `TF-A016 - Define external research cockpit import boundary`
- `TF-A017 - Implement research artifact ingestion API`
- `TF-A018 - Implement Codex/Claude-generated advisory artifact support`
- `TF-A019 - Implement advisory markdown artifact persistence`
- `TF-A020 - Implement replay-safe advisory snapshot capture`

M14C should therefore be treated as a planning and lifecycle-UX bridge on top of existing advisory artifact infrastructure, not as a greenfield ingestion capability.

## Key Concepts

- Research Cockpit output may become structured source material for TradeForge.
- The local import directory is the first proposed bridge between external cognition and TradeForge.
- Imported material should become a parsed draft artifact before review.
- Import Review is the proposed operator-facing workspace pattern.
- Promotion emits normal canonical lifecycle event types only after explicit operator approval.
- Source provenance must remain visible during replay.

Proposed flow:

```text
Research Cockpit / ChatGPT / Claude / Codex
    -> local import directory
    -> TradeForge import scanner
    -> parsed draft artifact
    -> Import Review workspace
    -> user edits / rejects / approves
    -> canonical lifecycle event
```

## UX Ideas

- Show imported draft material in an Import Review workspace.
- Allow the operator to edit imported content before promotion.
- Allow explicit reject and approve paths.
- Treat imported research as useful authorship support rather than as lifecycle authority.
- Preserve the operator's ability to write freely outside TradeForge while still using TradeForge as the controlled lifecycle system.

## Architectural Considerations

- Initial import types named in the note include:
  - thesis draft
  - plan draft
  - catalyst note
  - risk note
  - invalidation condition
  - evidence reference
  - advisory observation
  - watchlist candidate
- The suggested first slice is `TF-R001 - Import thesis draft from watched directory`.
- The imported artifact may be represented as `ImportedResearchDraft` or an equivalent derived/import artifact.
- Imported files should move to `/processed`, `/rejected`, or `/error` after review.
- Replay should show source provenance for promoted material.

## Canonical Boundary Concerns

The strongest boundary in the note is:

```text
external AI/research = advisory/source material
TradeForge = lifecycle authority
user = decision authority
event ledger = canonical truth
```

The importer must not directly create `TradeThesisCreated` or any other canonical lifecycle event. Doing so would risk violating Human Decision Sovereignty and lifecycle integrity.

## Stable Architectural Direction

- External research ingestion is useful and aligned with TradeForge if it remains advisory before review.
- Promotion must be explicit, operator-controlled, and routed through normal lifecycle events.
- Import provenance should be replay-visible.
- The first slice should remain narrow and deterministic.

## Unresolved Design Questions

- Should the import surface be a dedicated Import Review workspace or embedded inside lifecycle workspaces?
- What exact domain name should replace or formalize `ImportedResearchDraft`?
- Which import types belong in M14C versus later milestones?
- How much file movement state belongs in the runtime system versus local filesystem conventions?

## Risky Assumptions

- A local directory import flow will be sufficient for early operator workflows.
- Markdown/YAML structure will be reliable enough without AI parsing.
- Replay provenance can be preserved without prematurely making all imported artifacts event-backed.
- The operator will review imported content carefully enough to preserve decision authority.

## Doctrinal Implications

- Reinforces Human Decision Sovereignty.
- Reinforces advisory artifact boundaries.
- Reinforces the Event Ledger as canonical truth.
- Extends replayability doctrine to imported research provenance.
- Introduces a practical boundary between external cognition tools and TradeForge lifecycle authority.

## Open Questions

- What minimum provenance fields are required for replay?
- Should rejected imports remain reviewable later?
- Should import parsing failures become system events, diagnostics, or local error files only?
- What is the minimum schema needed before implementation?

## Future Extensions

- Plan draft imports.
- Catalyst, risk, invalidation, and evidence imports.
- Watchlist candidate imports.
- Advisory observation ingestion.
- Deeper Research Cockpit integration after the import boundary stabilizes.
