---
title: Runtime Planning Workflow Prompt
type: workflow-prompt
status: canonical
tags:
  - TradeForge
  - runtime-planning
  - issue-planning
  - architecture-first
  - replayability
  - semantic-governance
created: 2026-05-08
updated: 2026-05-08
prompt_scope: runtime-planning
related:
  - [[Runtime ↔ KB Development Loop]]
  - [[EXECUTION_CONTRACT]]
  - [[ARCHITECTURE]]
  - [[EVENT_TAXONOMY]]
  - [[INVARIANTS]]
---

# Runtime Planning Workflow Prompt

## Purpose

This prompt operationalizes architecture-first planning in the TradeForge runtime repository.

Use this prompt before implementing any runtime issue.

It exists to prevent:
- implementation-first coding
- issue scope drift
- missing ADRs
- roadmap/register drift
- lifecycle corruption
- event model mistakes
- semantic inconsistency between runtime and KB

Runtime planning must produce a bounded, reviewable implementation plan before code is written.

---

# Core Principle

Do NOT implement yet.

This prompt is for planning only.

Runtime planning must:
- load semantic context
- inspect runtime state
- verify roadmap/register alignment
- identify the correct next issue
- detect operational drift
- reason architecturally
- check ADR need
- produce an implementation plan
- produce a KB raw capture draft

Code generation happens only after planning has been captured and processed through the KB loop when appropriate.

---

# Invocation

Use in the runtime repository:

```text
C:\Users\bosto\dockerstuff\TradeForge
```

Suggested invocation:

```text
Use prompts/workflow/runtime-planning.md.

Follow the Runtime ↔ KB Development Loop playbook.

Plan the next issue in the current milestone.

Do not implement yet.
```

---

# Required Runtime Context

Before planning, inspect:

- `AGENTS.md`
- `README.md`
- `DOCS/`
- `DOCS/adr/`
- `DOCS/Milestone_Roadmap_v2.md`
- `DOCS/ISSUE_REGISTER.md`
- current git branch
- recent git history if milestone/issue state is unclear
- relevant `src/`
- relevant `tests/`

If roadmap/register state conflicts with git branch or implementation reality:
STOP and report operational state drift.

---

# Required KB Semantic Context

Before planning, ensure alignment with KB doctrine.

Required KB references:

- `knowledge-base/TradeForge/SEMANTIC_BOOTSTRAP.md`
- `knowledge-base/TradeForge/INVARIANTS.md`
- `knowledge-base/TradeForge/ARCHITECTURE.md`
- `knowledge-base/TradeForge/GLOSSARY.md`
- `knowledge-base/TradeForge/EVENT_TAXONOMY.md`
- `knowledge-base/TradeForge/EXECUTION_CONTRACT.md`

Optional depending on task:

- `knowledge-base/TradeForge/UX_DOCTRINE.md`
- `knowledge-base/TradeForge/PERSONAS.md`
- `knowledge-base/TradeForge/WORKSPACES.md`
- relevant KB skills
- relevant KB workflows
- relevant KB playbooks

If KB context is unavailable:
continue only with explicit uncertainty and do not make canonical claims.

---

# Planning Sequence

## 1. Establish Operational State

Determine:

- current branch
- current milestone
- current issue
- next planned issue
- completed issues
- open/deferred issues
- roadmap state
- issue register state

Compare:

- git branch
- git history
- runtime issue register
- runtime milestone roadmap
- KB issue register if present
- KB milestone roadmap if present

If disagreement exists, report:

```text
Operational state drift detected.
```

Then identify:
- what conflicts
- which source says what
- likely correction
- whether implementation should pause

Do not silently continue through drift.

---

## 2. Select Issue

Select the next issue only after operational state is coherent.

Issue selection must respect:

- milestone order
- issue register
- roadmap
- branch naming
- prior implementation state

If issue identity is ambiguous:
STOP and ask for operational state synchronization.

---

## 3. Define Scope

For the selected issue, define:

- issue objective
- explicit in-scope items
- explicit out-of-scope items
- expected runtime changes
- expected tests
- expected docs updates

Rules:
- keep scope narrow
- avoid opportunistic features
- avoid milestone creep
- avoid unrelated cleanup unless required

---

## 4. Activate Relevant Skills

Identify relevant KB/runtime skills.

Examples:

- event sourcing
- decision lifecycle
- execution discipline
- ontology drift detection
- replay analysis
- workspace querying

Skills act as constraints, not suggestions.

---

## 5. Architectural Interpretation

Before designing code, explain:

- affected bounded context
- affected lifecycle stage
- affected event categories
- affected canonical state
- affected derived state
- replay implications
- AI governance implications, if any
- human decision sovereignty implications, if any

Use TradeForge terminology consistently.

---

## 6. ADR Checkpoint

Evaluate whether an ADR is required.

ADR is required if the issue introduces or changes:

- architectural tradeoff
- invariant
- lifecycle semantics
- event model
- projection model
- replay behavior
- bounded context
- integration boundary
- persistence strategy
- source-of-truth boundary
- AI authority boundary
- durable future-facing decision

Output one of:

```text
ADR required: yes
Reason:
Proposed ADR:
```

or:

```text
ADR required: no
Reason:
```

Do not skip this section.

---

## 7. Event and Replay Check

If issue touches state or workflow, identify:

- event types involved
- whether new events are needed
- whether existing events are affected
- replay consequences
- projection consequences
- invalid event patterns to avoid

Rules:
- events are facts, not interpretations
- all meaningful state changes must be event-backed
- derived state is not canonical
- replay must not depend on live external APIs

---

## 8. Implementation Design Plan

Produce a concrete implementation plan.

Include:

- files likely to change
- modules/classes/functions affected
- data model changes
- event model changes
- service/workflow changes
- tests to add/update
- documentation updates
- validation commands

Do not write code yet.

---

## 9. KB Capture Draft

At the end of planning, produce a raw KB capture draft.

Suggested destination:

```text
knowledge/raw/YYYYMMDD-issue-topic-planning.md
```

Include:

- issue identifier
- milestone
- branch
- planning summary
- architecture reasoning
- ADR decision
- replay implications
- lifecycle implications
- implementation boundaries
- open questions
- follow-up notes

This capture should be copied into the KB and processed using:

```text
prompts/workflow/kb-processing.md
```

---

# Required Planning Output Format

Planning output MUST include:

```markdown
# Runtime Planning Result

## Operational State
## Selected Issue
## Scope
## Relevant Skills
## Architectural Interpretation
## ADR Checkpoint
## Event and Replay Check
## Implementation Design Plan
## Validation Plan
## Documentation Impact
## KB Raw Capture Draft
## Stop Point
```

The final section must say:

```text
Do not implement until this planning capture is processed or explicitly accepted.
```

---

# Anti-Patterns

Avoid:

- writing code during planning
- skipping roadmap/register verification
- ignoring branch mismatch
- treating raw notes as canonical
- assuming issue state from memory
- skipping ADR evaluation
- introducing new terminology casually
- changing lifecycle semantics silently
- using events for interpretations
- expanding scope beyond the issue
- hiding uncertainty

---

# Final Principle

Runtime planning exists to protect TradeForge from implementation drift.

Planning is complete only when:
- operational state is coherent
- issue scope is explicit
- architecture impact is understood
- ADR need is evaluated
- replay impact is understood
- implementation plan is bounded
- KB capture is ready for stabilization

Architecture first.

Implementation second.

Semantic stabilization always.
