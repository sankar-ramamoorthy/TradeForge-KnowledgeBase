---
title: Development Governance Synthesis — Issue Discipline + Knowledge Persistence
type: processed-synthesis
status: processed
created: 2026-05-15
source_history:
  - knowledge/raw/20260515-governance-issue-discipline-knowledge-persistence-thinking.md
  - knowledge/raw/20260515-tf-f-promotion-protocol-gap.md
  - knowledge/raw/notes on feedback-issue-development-loop.md
tags: [governance, development-discipline, issue-discipline, knowledge-persistence, playbook-gap]
related:
  - "[[SEMANTIC_GOVERNANCE]]"
  - "[[development-workflow-formalization]]"
---

# Development Governance Synthesis — Issue Discipline + Knowledge Persistence

## Stabilized Observations

### 1. Issue-First Development Discipline (from governance-issue-discipline note)

Canonical rule identified: no behavioral, architectural, workflow, or runtime code change may occur without a corresponding issue. Applies to all AI tools — Claude, Codex, and future agents — across both the KB repo and runtime repo.

**Knowledge persistence problem identified:** This rule lived only in personal memory files, making it invisible to Codex, fresh Claude sessions, and other AI tools. Rules must survive tool changes and cold starts.

**Proposed tier model:**
- Tier 1: `CLAUDE.md` / `AGENTS.md` — boot-time pointer (small, references doctrine)
- Tier 2: `DEVELOPMENT_GOVERNANCE.md` at KB root — full canonical rule text
- Tier 3: ADRs — decision-level traceability (candidate: ADR-0024)
- Tier 5: Personal memory files — session-only ephemera (NOT for rules)

**Status at capture time:** Raw thinking, not yet canonicalized. Open questions: CLAUDE.md size constraint, root vs index placement, progressive discovery vs always-load.

### 2. TF-F Promotion Protocol Gap (from tf-f-promotion-protocol-gap note)

When a TF-F issue graduates into a formal milestone (roadmap entry, ADR, multi-layer scope), neither existing playbook covers the handoff:
- `feedback-issue-development-loop` assumes reactive, lighter KB cycle
- `runtime-kb-development-loop` assumes TF-#### issues born from milestone planning

**Interim rule documented:** TF-F issues with milestone assignment, roadmap entry, or ADR start at Phase 3 of the runtime loop. Phases 1–2 of the feedback loop (triage + diagnosis) are complete. Feedback closure (Phase 9) still applies at implementation end.

**Playbook gaps requiring update:** `feedback-issue-development-loop.md` needs a graduation section; `runtime-kb-development-loop.md` needs a note that TF-F milestoned issues are valid work items.

### 3. Feedback Issue Development Loop Design (from notes-on-feedback-loop note)

Separate feedback loop warranted because the 12-phase milestone loop is over-specified for reactive fixes. Key structural differences needed:
- Phase 2: Diagnosis before planning (not in milestone loop)
- Phase 3 ADR: conditional on classification, not always mandatory
- Phase 7: Explicit regression check
- Phase 9: Feedback loop closure against original observation
- Lighter KB cycle: one processed synthesis, not full stabilization

**Proposed outputs:** `DOCS/feedback-issue-workflow.md` (runtime repo, architectural rationale) + `playbooks/development/feedback-issue-development-loop.md` (KB repo, operational playbook).

## Unresolved at Capture Time

- DEVELOPMENT_GOVERNANCE.md scope and placement
- AGENTS.md need
- Progressive discovery model for development governance context
- Playbook updates pending implementation
