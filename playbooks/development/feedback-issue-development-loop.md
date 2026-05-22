---
title: Feedback Issue Development Loop
type: playbook
status: canonical
tags:
  - TradeForge
  - development-loop
  - feedback-workflow
  - bug-fixing
  - operational-testing
  - diagnosis
  - TF-F
created: 2026-05-15
updated: 2026-05-15
canonical_location: playbooks/development/feedback-issue-development-loop.md
issue_series: TF-F###
see_also:
  - playbooks/development/runtime-kb-development-loop.md
  - DOCS/feedback-issue-workflow.md
related:
  - "[[INVARIANTS]]"
  - "[[Decision Lifecycle]]"
  - "[[Replayability Is Foundational]]"
  - "[[UX Is Architectural]]"
  - "[[Human Decision Sovereignty]]"
  - "[[SEMANTIC_GOVERNANCE]]"
  - "[[CodexSkill]]"
  - "[[WorkflowPrompt]]"
---

# Feedback Issue Development Loop

## Purpose

This playbook defines the canonical development workflow for TradeForge feedback-originated issues (`TF-F###`).

Feedback issues originate from:
- operational walkthroughs
- testing sessions
- live system observation
- architectural realizations discovered during use

This loop is distinct from the [[Runtime ↔ KB Development Loop]], which governs milestone feature work.

The key structural difference: this loop front-loads **diagnosis** before planning. You do not know what to build until you know what is actually wrong and why.

---

# Core Principle

This workflow answers:

> "What is actually broken or missing — and what is the minimum correct change?"

Not:

> "What is the next thing to build?"

Implementation without diagnosis is prohibited.

---

# Issue Series Reminder

| Series | Pattern | Governed By |
|---|---|---|
| Roadmap issues | `TF-####` | Runtime ↔ KB Development Loop |
| Feedback issues | `TF-F###` | This playbook |

---

# Required Context Initialization

Before any work begins, load:

- [[INVARIANTS]]
- [[Decision Lifecycle]] — if the issue touches lifecycle semantics
- [[UX Is Architectural]] — if the issue touches interaction design
- [[Replayability Is Foundational]] — if the issue touches event or replay behavior
- `DOCS/ISSUE_REGISTER.md` — to confirm TF-F issue details and classification
- Source feedback material (raw note, screenshots) — mandatory

Do not load the full KB. Load only what is relevant to the classified gap.

If semantic context has not been loaded:

**STOP.**

---

# Feedback Loop — Phase by Phase

---

## Phase 1 — Triage and Select Issue

Select the TF-F issue to work on.

Steps:

1. Review `DOCS/ISSUE_REGISTER.md` — TF-F series
2. Confirm issue classification: `bug` | `enhancement` | `architectural` | `doctrine` | `refactor` | `operational`
3. Locate and read the source feedback material:
   - raw note in `knowledge/raw/`
   - screenshots or observations referenced in the issue
4. Read the processed synthesis if one exists in `knowledge/processed/`

Rules:
- never begin work without reading the original source feedback
- classification determines which later phases are mandatory
- do not reinterpret the issue in isolation from its source

---

## Phase 2 — Diagnosis

Understand the root cause before forming any implementation approach.

Diagnosis answers:

- What is the observable gap?
- What is the architectural or implementation cause?
- Which invariants are relevant?
- What is the minimum correct scope of change?
- What must remain explicitly out of scope?

Diagnosis must be written down — not held in working memory.

Capture diagnosis into:

```text
knowledge/raw/YYYYMMDD-tf-fNNN-diagnosis.md
```

Rules:
- diagnosis precedes planning — no exceptions
- scope containment is mandatory output of diagnosis
- if root cause is unclear, do not proceed to planning

---

## Phase 3 — ADR Checkpoint

Evaluate whether an ADR is required before implementation.

**Mandatory** when the fix involves:
- a new or changed lifecycle state
- event model changes
- domain model structural changes
- invariant boundary changes
- new bounded contexts
- durable architectural decisions with future-facing implications

**Optional** when the fix is:
- a UI/UX enhancement within existing architecture
- a bug fix with no architectural implications
- an ergonomic or configuration change

Trigger: issue classification is `architectural`.

If an ADR is required, create or update it in:

```text
TradeForge/DOCS/adr/
```

before proceeding to implementation.

Rules:
- missing ADRs on architectural feedback issues create the same drift as missing ADRs on feature issues
- ADR evaluation is a gate, not a formality

---

## Phase 4 — Scope and Approach

Define the implementation approach based on diagnosis.

Planning must include:
- what will change and why
- affected modules and layers
- event implications (if any)
- replay implications (if any)
- lifecycle implications (if any)
- what is explicitly out of scope
- testing approach
- regression risk areas

Rules:
- scope must remain bounded to the diagnosed gap
- do not extend scope to adjacent improvements unless a new TF-F issue exists for them
- architecture interpretation precedes code generation

---

## Phase 5 — KB Planning Capture (Lightweight)

Capture planning reasoning into:

```text
knowledge/raw/YYYYMMDD-tf-fNNN-planning.md
```

Contents:
- diagnosed root cause summary
- chosen approach and rationale
- scope boundary
- replay or lifecycle implications
- open questions

Rules:
- raw captures are non-canonical
- preserve reasoning context even if the approach changes
- this is lighter than milestone planning captures — one focused note is sufficient

---

## Phase 6 — Implement

Implement the scoped fix.

Required activities:
- implement only what diagnosis and planning defined
- write tests covering the fixed behavior
- verify replay safety if event behavior is touched
- verify lifecycle integrity if lifecycle behavior is touched

Rules:
- bounded scope is mandatory
- do not fix adjacent issues in the same commit — create new TF-F issues for them
- deterministic authority remains canonical
- avoid ontology drift

---

## Phase 7 — Regression Check

Explicitly verify that the fix did not break surrounding behavior.

Regression check must cover:
- adjacent workflows touched by the changed layer
- tests that existed before the fix
- any invariants that are relevant to the changed code path

This is a mandatory phase — not implicit in testing.

Run:

```text
uv run pytest
npm run typecheck
npm run build
```

If regressions are found: diagnose and fix before proceeding. Do not defer.

---

## Phase 8 — KB Processed Note

Create or update a processed synthesis note.

File:

```text
knowledge/processed/YYYYMMDD-tf-fNNN-synthesis.md
```

Contents:
- what the feedback identified
- root cause as diagnosed
- what was changed and why
- invariants involved
- replay or lifecycle implications
- link to source raw feedback
- link to TF-F issue ID

Frontmatter must include:

```yaml
---
title: TF-FXXX Synthesis — [Short Title]
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, ...]
created: YYYY-MM-DD
updated: YYYY-MM-DD
source_history:
  - knowledge/raw/[source feedback note]
  - knowledge/raw/[diagnosis note]
  - knowledge/raw/[planning note]
related:
  - "[[relevant invariant or entity]]"
issues:
  - TF-FXXX
---
```

Rules:
- one processed note per feedback issue is sufficient for most cases
- full topic or entity promotion is only warranted if the issue surfaces a genuinely new architectural concept
- the processed note is the canonical record of the resolution

---

## Phase 9 — Feedback Loop Closure

Close the loop against the original observation.

Steps:

1. Verify the fix resolves the specific gap described in the source feedback
2. If the original feedback came from a walkthrough or test session, confirm the scenario that failed now behaves correctly
3. If follow-on gaps were discovered during the fix, create new TF-F issues for them — do not absorb them silently
4. Update the TF-F issue in `DOCS/ISSUE_REGISTER.md` with resolution notes

Rules:
- closure is not the same as code merge
- closure requires explicit verification against the original source feedback
- undocumented follow-on gaps are architectural drift

---

## Phase 10 — Operational Sync

Update operational state.

Mandatory updates:

- `DOCS/ISSUE_REGISTER.md` — mark TF-F issue as Done, add resolution summary
- `Milestone_Roadmap_v2.md` — only if the issue was formally assigned to a milestone

Rules:
- stale issue register state is architectural drift
- do not mark Done until Phase 9 (feedback loop closure) is complete

---

## Phase 11 — Commit Runtime Repository

Commit:
- implementation
- tests
- ADR (if created)
- issue register update

Commit message must reference the TF-F issue ID.

Example:

```
TF-F002: introduce awaiting-trigger lifecycle state between approval and execution
```

---

## Phase 12 — Commit KB Repository

Commit:
- diagnosis raw note
- planning raw note
- processed synthesis note
- any entity or topic promotions

Rules:
- KB commits preserve operational reasoning
- processed note must be committed before the runtime commit is considered complete

---

# Conditional Phase Summary

| Phase | Bug | Enhancement | Architectural |
|---|---|---|---|
| 1 — Triage | Mandatory | Mandatory | Mandatory |
| 2 — Diagnosis | Mandatory | Mandatory | Mandatory |
| 3 — ADR Checkpoint | Optional | Optional | **Mandatory** |
| 4 — Scope and Approach | Mandatory | Mandatory | Mandatory |
| 5 — KB Planning Capture | Mandatory | Mandatory | Mandatory |
| 6 — Implement | Mandatory | Mandatory | Mandatory |
| 7 — Regression Check | **Mandatory** | Mandatory | Mandatory |
| 8 — KB Processed Note | Mandatory | Mandatory | Mandatory |
| 9 — Feedback Loop Closure | Mandatory | Mandatory | Mandatory |
| 10 — Operational Sync | Mandatory | Mandatory | Mandatory |
| 11 — Commit Runtime | Mandatory | Mandatory | Mandatory |
| 12 — Commit KB | Mandatory | Mandatory | Mandatory |

---

# When a TF-F Issue Becomes a Milestone Feature

If diagnosis reveals the gap is too significant for a single bounded fix:
- the TF-F issue is not closed
- a new roadmap entry and TF-#### issue are created to govern the larger work
- the TF-F issue is linked as the source observation
- the milestone loop governs implementation from that point

Example: TF-F002 (awaiting trigger lifecycle state) may be promoted into a full milestone feature if the lifecycle architecture requires significant restructuring.

---

# Final Checklist

Before considering a feedback issue complete:

- [ ] Source feedback material read
- [ ] Issue classification confirmed
- [ ] Diagnosis completed and captured in raw note
- [ ] Root cause identified — not just symptom treated
- [ ] Scope boundary defined and respected
- [ ] ADR evaluated (created if architectural)
- [ ] Implementation bounded to diagnosed scope
- [ ] Regression check passed
- [ ] Processed KB note created with source traceability
- [ ] Feedback loop closed against original observation
- [ ] Follow-on gaps captured as new TF-F issues
- [ ] ISSUE_REGISTER updated — TF-F marked Done
- [ ] Runtime repository committed with TF-F reference
- [ ] KB repository committed

---

# Anti-Patterns

Avoid:
- implementing without diagnosis
- treating symptoms rather than root causes
- absorbing adjacent issues into the same fix
- skipping the regression check
- closing the TF-F issue without verifying against source feedback
- skipping the processed KB note
- creating TF-F issues without reading the source observation
- scope creep under the justification of "while I'm in here"

---

# Closing Principle

Feedback issues represent the system revealing its own invariant boundaries under real operational use.

The feedback loop exists to ensure gaps are:
- diagnosed correctly at root cause
- resolved at the minimum correct scope
- preserved as replayable operational knowledge
- closed against the observation that surfaced them

Correctness and traceability remain more important than resolution speed.
