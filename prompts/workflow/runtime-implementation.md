---
title: Runtime Implementation Workflow Prompt
type: workflow-prompt
status: canonical
tags:
  - TradeForge
  - runtime-implementation
  - implementation
  - replayability
  - event-sourcing
  - architecture-governance
created: 2026-05-08
updated: 2026-05-08
prompt_scope: runtime-implementation
related:
  - [[Runtime ↔ KB Development Loop]]
  - [[EXECUTION_CONTRACT]]
  - [[ARCHITECTURE]]
  - [[EVENT_TAXONOMY]]
  - [[INVARIANTS]]
---

# Runtime Implementation Workflow Prompt

## Purpose

This prompt operationalizes disciplined runtime implementation for TradeForge.

Use this prompt AFTER:
- runtime planning is complete
- operational state is coherent
- ADR evaluation is complete
- planning capture exists
- KB stabilization has occurred when appropriate

This prompt exists to prevent:
- uncontrolled implementation
- architecture drift
- lifecycle corruption
- replayability violations
- event model corruption
- semantic inconsistency
- scope creep
- hidden assumptions

Implementation must remain:
- bounded
- replay-safe
- event-aware
- architecture-aligned
- semantically consistent

---

# Core Principle

Implementation is NOT:
- exploratory coding
- uncontrolled refactoring
- opportunistic architecture redesign

Implementation IS:
- execution of an approved architectural plan
- bounded realization of explicit issue scope
- deterministic translation of architecture into runtime systems

The runtime repository implements architecture.

It does not invent architecture casually.

---

# Invocation

Use in the runtime repository:

```text
C:\Users\bosto\dockerstuff\TradeForge
```

Suggested invocation:

```text
Use prompts/workflow/runtime-implementation.md.

Implement the approved issue plan.

Preserve replayability, lifecycle integrity, and event integrity.
```

---

# Preconditions

Before implementation begins, verify:

- operational state is synchronized
- issue selection is correct
- roadmap state is coherent
- issue register state is coherent
- planning phase completed
- ADR checkpoint completed
- KB capture exists
- semantic initialization completed

If any precondition is missing:
STOP.

---

# Required Runtime Context

Inspect:

- `AGENTS.md`
- relevant ADRs
- `DOCS/`
- selected issue
- milestone roadmap
- issue register
- relevant source modules
- relevant tests
- relevant projections
- relevant event models

---

# Required KB Semantic Context

Ensure alignment with:

- `SEMANTIC_BOOTSTRAP.md`
- `INVARIANTS.md`
- `ARCHITECTURE.md`
- `GLOSSARY.md`
- `EVENT_TAXONOMY.md`
- `EXECUTION_CONTRACT.md`

Optional:
- `WORKSPACES.md`
- `PERSONAS.md`
- relevant KB workflows
- relevant KB entities
- relevant KB topics
- relevant skills

Implementation must not violate KB doctrine.

---

# Implementation Responsibilities

---

# 1. Preserve Issue Scope

Implement ONLY:
- approved scope
- approved architecture changes
- approved lifecycle changes

Do NOT:
- opportunistically redesign systems
- introduce unrelated abstractions
- add unrelated features
- silently expand milestone scope

If new requirements emerge:
STOP and return to planning.

---

# 2. Preserve Replayability

Implementation must preserve replayability.

Verify:
- canonical state remains event-backed
- state reconstruction remains deterministic
- projections remain derivable
- replay logic remains stable
- replay does not depend on live APIs

Never introduce:
- hidden mutable state
- replay-breaking shortcuts
- non-deterministic workflow behavior

Replayability is foundational.

---

# 3. Preserve Lifecycle Integrity

If implementation affects workflow state:

Verify:
- lifecycle transitions remain explicit
- invalid transitions remain impossible
- lifecycle rules remain deterministic
- workflow authority remains centralized
- hidden state transitions do not emerge

The lifecycle engine remains authoritative.

---

# 4. Preserve Event Integrity

If implementation affects events:

Verify:
- events remain immutable facts
- event naming remains canonical
- event semantics remain stable
- events are not interpretations
- all meaningful state changes remain event-backed

Avoid invalid event patterns such as:
- emotional interpretation
- inferred confidence
- ambiguous semantic meaning

Event design is system design.

---

# 5. Preserve Source-of-Truth Boundaries

Respect architectural ownership boundaries.

Canonical truth:
- Event Ledger
- lifecycle state
- deterministic workflow transitions

Derived state:
- projections
- rankings
- summaries
- UI surfaces

AI systems:
- advisory only
- never authoritative

Do not collapse:
- canonical state
- derived state
- inferred state

---

# 6. Preserve AI Governance Boundaries

AI systems may:
- summarize
- rank
- contextualize
- surface opportunities

AI systems may NOT:
- mutate canonical state
- approve lifecycle transitions
- bypass workflow controls
- fabricate events
- become authoritative

Human decision sovereignty remains foundational.

---

# 7. Maintain Terminology Discipline

Use canonical terminology consistently.

Preferred terminology:
- Event Ledger
- Decision Lifecycle Engine
- Replay Session
- Persona Workspace
- Decision Surface
- Scenario Discovery

Avoid:
- alternate terminology
- casual renaming
- silent semantic mutation

If terminology evolution is required:
STOP and escalate through planning/ADR review.

---

# 8. Validate Architectural Boundaries

Respect bounded contexts.

Examples:
- Personas shape reasoning context
- Workspaces shape operational cognition
- Scenario systems are advisory
- Lifecycle Engine owns workflow authority
- Event Ledger owns canonical truth
- Replay system reconstructs history

Avoid:
- cross-boundary ownership violations
- leaking workflow authority
- bypassing event flows

---

# 9. Testing Responsibilities

Implementation must include validation.

Expected validation:
- unit tests
- workflow tests
- replay tests when applicable
- event integrity checks
- lifecycle validation
- regression checks

Suggested validation commands:

```text
python -m compileall src tests scripts
uv run pytest
```

If tests fail:
STOP and resolve before completion.

---

# 10. Documentation Responsibilities

Update runtime docs when appropriate.

Potential updates:
- ADRs
- roadmap
- issue register
- architecture notes
- README
- workflow docs

Documentation drift is architectural drift.

---

# 11. Operational State Synchronization

Before implementation is considered complete:

Verify:
- `DOCS/Milestone_Roadmap_v2.md`
- `DOCS/ISSUE_REGISTER.md`

Ensure:
- completed work marked correctly
- milestone state accurate
- deferred work documented
- issue status synchronized
- branch state aligned with issue state

If mismatch exists:
report explicitly.

Do not silently continue.

---

# 12. KB Capture Preparation

At implementation completion, prepare a KB raw capture.

Suggested destination:

```text
knowledge/raw/YYYYMMDD-implementation-topic.md
```

Include:
- issue identifier
- milestone
- branch
- implementation summary
- files changed
- tests executed
- replay implications
- lifecycle implications
- event implications
- unresolved concerns
- follow-up opportunities
- architecture observations

This capture should later be processed using:

```text
prompts/workflow/kb-processing.md
```

---

# Required Implementation Output Format

Implementation output SHOULD include:

```markdown
# Runtime Implementation Result

## Issue
## Scope Verification
## Architectural Constraints Preserved
## Files Changed
## Event Impact
## Replay Impact
## Lifecycle Impact
## Tests Executed
## Documentation Updates
## Operational Synchronization Status
## KB Capture Draft
## Remaining Concerns
```

---

# Stop Conditions

STOP implementation if:

- roadmap/register state is inconsistent
- branch state conflicts with issue state
- lifecycle semantics become unclear
- replayability would be compromised
- event ownership becomes ambiguous
- issue scope expands materially
- ADR need emerges unexpectedly
- terminology drift appears
- canonical truth boundaries blur

Return to planning instead.

---

# Anti-Patterns

Avoid:
- implementation-first reasoning
- silent architecture mutation
- hidden state transitions
- bypassing event flows
- direct mutable canonical state
- replay shortcuts
- opportunistic feature additions
- undocumented assumptions
- semantic drift
- unbounded refactors
- AI authority escalation

---

# Final Principle

Runtime implementation exists to translate stabilized architecture into deterministic executable systems.

Implementation is successful only when:
- replayability is preserved
- lifecycle integrity is preserved
- event integrity is preserved
- operational state is synchronized
- architecture boundaries remain intact
- semantic consistency remains stable

Architecture governs implementation.

Replayability governs architecture.

Semantic stability governs everything.
