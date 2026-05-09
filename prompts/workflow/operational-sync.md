---
title: Operational Synchronization Workflow Prompt
type: workflow-prompt
status: canonical
tags:
  - TradeForge
  - operational-sync
  - replayability
  - milestone-governance
  - issue-governance
  - architecture-governance
created: 2026-05-08
updated: 2026-05-08
prompt_scope: operational-synchronization
related:
  - [[Runtime ↔ KB Development Loop]]
  - [[ARCHITECTURE]]
  - [[EXECUTION_CONTRACT]]
  - [[INVARIANTS]]
---

# Operational Synchronization Workflow Prompt

## Purpose

This prompt operationalizes operational state synchronization for TradeForge.

It exists to ensure that:
- implementation reality
- roadmap state
- issue register state
- branch state
- ADR state
- KB references

remain synchronized and replayable.

This prompt exists to prevent:
- roadmap drift
- issue register drift
- milestone ambiguity
- replay failures
- hidden implementation progress
- architecture state confusion
- semantic inconsistencies between KB and runtime

Operational synchronization is mandatory before work is considered complete.

---

# Core Principle

Implementation is NOT complete when:
- code compiles
- tests pass
- features work

Implementation is complete only when:
- operational tracking reflects reality
- milestone state is coherent
- issue state is synchronized
- KB references remain aligned
- replayability of development state is preserved

Operational state is part of canonical project cognition.

---

# Invocation

Use in the runtime repository:

```text
C:\Users\bosto\dockerstuff\TradeForge
```

Suggested invocation:

```text
Use prompts/workflow/operational-sync.md.

Perform operational state synchronization before completion.
```

---

# Required Runtime Context

Inspect:

- current git branch
- recent git history
- `DOCS/MILESTONE_ROADMAP.md`
- `DOCS/ISSUE_REGISTER.md`
- relevant ADRs
- current implementation state
- recently completed issues
- pending/open issues

Optional:
- KB issue register
- KB roadmap mirrors
- KB planning captures
- KB implementation captures

---

# Synchronization Responsibilities

---

# 1. Verify Current Operational State

Determine:

- current milestone
- active issue
- completed issues
- planned issues
- deferred issues
- branch naming alignment
- merge status
- milestone progression

Compare:
- roadmap
- issue register
- git branch
- recent commits
- actual implementation state

---

# 2. Detect Operational Drift

Identify mismatches between:
- roadmap state
- issue register
- git history
- implementation reality
- KB references

Examples:
- issue marked planned but implemented
- branch references non-existent issue state
- milestone advanced but roadmap unchanged
- KB roadmap diverges from runtime roadmap
- ADR exists but issue register missing linkage

If drift exists:
report explicitly.

Use:

```text
Operational state drift detected.
```

Then explain:
- what conflicts
- which source says what
- likely authoritative correction
- recommended synchronization action

Never silently continue through drift.

---

# 3. Synchronize Issue Register

Verify:
- completed issues marked correctly
- issue ordering preserved
- deferred work documented
- current issue status accurate
- branch naming aligned
- implementation reflected accurately

Potential statuses:
- Planned
- In Progress
- Done
- Deferred
- Blocked
- Cancelled

Rules:
- avoid ambiguous state
- avoid hidden completion
- avoid stale issue ownership

---

# 4. Synchronize Milestone Roadmap

Verify:
- milestone progress accurate
- completed milestone work reflected
- deferred scope documented
- implementation state matches roadmap state
- roadmap sequencing remains coherent

Rules:
- roadmap must reflect implementation reality
- roadmap is a replayable planning artifact
- stale roadmap state is replay failure

---

# 5. Verify ADR Alignment

Verify:
- ADR references still valid
- issue references align with ADRs
- architecture decisions reflected operationally
- missing ADRs surfaced explicitly

Questions:
- was architecture changed without ADR?
- does roadmap imply architecture changes missing ADR coverage?
- does implementation contradict documented ADRs?

---

# 6. Verify Branch Alignment

Verify:
- branch naming reflects active issue/milestone
- merge history aligns with operational documents
- branch progression matches roadmap progression

Detect:
- orphaned branches
- stale milestone branches
- implementation on wrong branch
- inconsistent milestone transitions

---

# 7. Verify KB Alignment

If KB mirrors exist, verify alignment with:
- runtime issue register
- runtime roadmap
- implementation state
- milestone progression

Potential KB locations:
- `knowledge/index/ISSUE_REGISTER.md`
- `knowledge/index/MILESTONE_ROADMAP.md`

If KB drift exists:
surface explicitly.

Do not silently assume runtime authority solved the problem.

---

# 8. Verify Replayability of Development State

Development replayability requires future agents to reconstruct:
- what was implemented
- when milestone state changed
- why issue state evolved
- what ADRs governed changes
- how implementation progression occurred

Verify:
- issue progression remains reconstructable
- milestone evolution remains reconstructable
- operational state changes are explicit
- architectural evolution is traceable

---

# 9. Recommend Synchronization Actions

When inconsistencies exist, recommend:
- roadmap updates
- issue register updates
- KB mirror updates
- ADR additions
- milestone corrections
- branch cleanup
- documentation alignment

Recommendations should be:
- explicit
- minimally destructive
- replay-preserving

Avoid:
- silent rewriting of history
- collapsing historical state
- hiding milestone evolution

---

# 10. Completion Verification

Before completion, verify:

- roadmap synchronized
- issue register synchronized
- milestone state coherent
- branch state coherent
- ADR references coherent
- KB references coherent
- implementation state reflected accurately

Only then may work be considered operationally complete.

---

# Required Output Format

Synchronization output SHOULD include:

```markdown
# Operational Synchronization Result

## Current Operational State
## Current Milestone
## Current Issue
## Completed Issues
## Planned Issues
## Drift Detection
## Roadmap Synchronization
## Issue Register Synchronization
## ADR Alignment
## Branch Alignment
## KB Alignment
## Replayability Assessment
## Recommended Corrections
## Final Synchronization Status
```

---

# Stop Conditions

STOP and escalate if:
- milestone identity is ambiguous
- issue sequencing is corrupted
- roadmap contradicts implementation reality
- branch history conflicts with issue history
- ADR state becomes unclear
- KB and runtime diverge materially
- replayability of development state is compromised

Do not continue implementation until operational state becomes coherent.

---

# Anti-Patterns

Avoid:
- stale issue registers
- stale roadmap state
- hidden milestone advancement
- silent implementation completion
- undocumented architecture evolution
- inconsistent branch naming
- ambiguous milestone ownership
- rewriting historical planning state without explanation
- collapsing development history

---

# Final Principle

Operational synchronization preserves replayable development cognition.

The roadmap, issue register, ADRs, branch history, and KB references together form:

- development memory
- milestone history
- architecture evolution history
- replayable operational state

TradeForge development must remain:
- reconstructable
- explainable
- synchronized
- semantically coherent

Operational drift is architectural drift.