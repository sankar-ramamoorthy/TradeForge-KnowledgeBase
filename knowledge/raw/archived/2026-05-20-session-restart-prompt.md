---
title: Session Restart Prompt — 2026-05-21
type: session-restart
status: raw
created: 2026-05-20
purpose: Orient the next session with full context of where things stand after the 2026-05-20 working session.
---

# Session Restart Prompt — 2026-05-21

## What Was Done Today (2026-05-20)

### KB Processing — Raw Folder Cleared

All notes in `knowledge/raw/` have been processed and archived. The folder is now empty except for this file.

**What was done:**
- 130+ raw notes moved to `knowledge/raw/archived/` across 5 archiving batches
- 9 new processed synthesis files created covering:
  - M9 market data subsystem (TF-0043, TF-0044, TF-0051, TF-0052)
  - M10 UX continuity (TF-0054/55/56/57 combined)
  - M10A structured cognition sub-issues (AIS03–AIS12)
  - M10D provider capability architecture
  - Development governance thinking
  - Context workbench complexity assessment (TF-F038 context)
- 17 existing processed/topic pages updated with `source_history` fields that were missing
- `knowledge/raw/archived/` now contains all historical raw notes with full traceability

### KB Processing Rule 11 Updated

`prompts/workflow/kb-processing.md` Rule 11 was changed from **delete** to **archive**. Raw notes are now moved to `knowledge/raw/archived/` instead of being deleted. This is already the practice (53 files were already in archived before today).

### Skills Updated

All skills in `skills/` were updated:
- Added YAML front matter (`id`, `title`, `category`, `applies_to`, `version`, `description`) to all 7 skill files
- Replaced "Codex" with "Agent" throughout so Claude and Codex are both governed
- `SKILL.md` updated to reflect agent-agnostic language
- `skills/ontology/structure-feedback-note.md` updated to say "archiving" not "deletion" (aligning with Rule 11)

All changes committed and pushed to `origin/M13`.

---

## Current Milestone State

### M11 — AI Advisory Boundary

**Status: DONE** — both runtime and KB.

Runtime issues all complete:
- TF-0065: Define AI advisory interfaces (**Done**)
- TF-0066: Implement replay summarization assistance (**Done**)
- TF-0067: Implement review assistance (**Done**)
- TF-0068: Implement advisory provenance tracking (**Done**)
- TF-F044: Fold machine-assisted discretionary cognition roadmap into v2 (**Done**)

KB capture exists: `knowledge/processed/20260519-m11-ai-advisory-boundary-synthesis.md`

No outstanding M11 items.

---

### M12 — Advisory Observation And Cognitive Evidence Layer

**Status: PLANNED — runtime implementation has NOT started.**

All TF-A001 through TF-A020 are `Planned` in `DOCS/ISSUE_REGISTER.md`.

**KB semantic work: done on this branch.**
The commit `4428401 Define M12 advisory cognition semantics` captured the M12 semantic foundation in the KB. This is preparatory knowledge-base work only — it does not mean M12 runtime work has started.

**Gap to be aware of:** The issue register only has detailed specs for TF-A001 through TF-A005. TF-A006 through TF-A020 appear in the index table but do not have full detail sections (Problem, Affected Layer, Linked ADRs, Acceptance Criteria). These need to be written before M12 implementation work begins.

**Core M12 concept:** Advisory Observation And Cognitive Evidence Layer — machine-assisted observations become durable advisory artifacts without lifecycle or execution authority. Separation of advisory space (Observation, Signal, Context, Candidate) from commitment space (Thesis, Plan, Approval, Execution) is the foundational M12 invariant.

**Runtime branch placeholder:** `feature/m12-advisory-observation-foundation`

---

### M13 — Contextual Interpretation And Thesis Influence

**Status: PLANNED — runtime implementation has NOT started.**

All TF-B001 through TF-B015 are `Planned` in `DOCS/ISSUE_REGISTER.md`.

**KB semantic work: done on this branch.**
The commit `d7bf01e Define M13 contextual interpretation semantics` captured the M13 semantic foundation in the KB. Again, this is KB preparatory work only.

**Gap to be aware of:** Like M12, TF-B001 through TF-B015 appear in the issue register index table but do not have full detail sections yet. These need to be written before M13 implementation begins. M12 must be fully implemented before M13 begins.

**Core M13 concept:** Raw advisory observations gain contextual meaning through an interpretation layer. Models the chain: Observation → Interpretation → Contextual Weighting → Thesis Influence. Explicitly excludes deterministic scoring, autonomous recommendations, and opaque ranking systems.

**Runtime branch placeholder:** `feature/m13-contextual-interpretation-thesis-influence`

---

## What "KB M13 Branch Done" Means

The current KB git branch is `M13`. This branch was for **KB-level semantic groundwork** covering M11 closeout, M12 semantic definitions, M13 semantic definitions, KB processing, and skill updates.

**That KB branch work is now complete.** It should be reviewed for PR merge into KB `main`.

The name "M13 branch" does NOT mean M12 or M13 runtime implementation is done. M12 and M13 are genuinely future work in the runtime repository.

---

## Immediate Next Steps (in priority order)

1. **Review KB M13 branch for PR merge** — the branch covers M11 closeout, M12/M13 semantic definitions, full KB processing, and skill updates. Create PR from `M13` → `main` in the KB repo.

2. **Write detailed issue specs for TF-A006 through TF-A020** — these M12 issues exist in the index but don't have full spec detail. Before M12 implementation starts, each issue needs Problem, Affected Layer, Linked ADRs, Impacted Invariants, and Acceptance Criteria sections in `DOCS/ISSUE_REGISTER.md`.

3. **Write detailed issue specs for TF-B001 through TF-B015** — same situation for M13. However, M12 must be done first.

4. **Begin M12 runtime implementation** — start with TF-A001 (AdvisoryObservation domain model) and work through the M12 issue sequence. The runtime branch is `feature/m12-advisory-observation-foundation`.

---

## Key Files To Load For Context

For M12/M13 semantic context (KB):
- `knowledge/processed/20260519-machine-assisted-discretionary-cognition-synthesis.md`
- Runtime roadmap M12 and M13 sections: `DOCS/Milestone_Roadmap_v2.md` (lines ~2109–2265)
- `DOCS/ISSUE_REGISTER.md` — TF-A001 through TF-A005 have full specs; TF-A006+ are stubs

For current KB state:
- `knowledge/index/MILESTONE_ROADMAP.md` — KB index of all milestones
- `knowledge/index/ISSUE_REGISTER.md` — KB semantic index
- `skills/SKILL.md` — agent skill system (updated today)

---

## Architectural Invariant Reminders For M12/M13 Work

- AI output is advisory only. It cannot mutate canonical state, transition lifecycle stages, or approve decisions.
- Advisory observations belong in advisory space, not commitment space.
- All advisory artifacts must carry provenance, uncertainty metadata, and capture origin.
- Advisory artifacts stored outside the event ledger (non-canonical persistence), but the *fact* that an observation was captured is a canonical event (`advisory.observation_captured`).
- Human decision sovereignty is non-negotiable throughout M12 and M13.
