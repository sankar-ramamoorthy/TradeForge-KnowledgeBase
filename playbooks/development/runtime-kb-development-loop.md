---
title: Runtime ↔ KB Development Loop
type: playbook
status: canonical
tags:
  - TradeForge
  - development-loop
  - runtime
  - knowledge-base
  - replayability
  - semantic-stabilization
  - architecture-governance
created: 2026-05-08
updated: 2026-05-09
canonical_location: playbooks/development/runtime-kb-development-loop.md
related:
  - [[ARCHITECTURE]]
  - [[EVENT_TAXONOMY]]
  - [[EXECUTION_CONTRACT]]
  - [[INVARIANTS]]
  - [[WORKSPACES]]
  - [[Decision Lifecycle]]
  - [[Event Ledger]]
  - [[Replay Session]]
  - [[CodexSkill]]
  - [[WorkflowPrompt]]
---

# Runtime ↔ KB Development Loop

## Purpose

This playbook defines the canonical development workflow used for TradeForge.

TradeForge development operates across TWO independent but connected systems:

1. Runtime repository
2. Knowledge Base repository

The runtime repository contains executable implementation.

The Knowledge Base (KB) contains:
- semantic stabilization
- ontology governance
- architecture doctrine
- workflows
- playbooks
- prompts
- reusable reasoning structures
- replayable project cognition

This workflow exists to preserve:
- replayability
- semantic consistency
- lifecycle integrity
- ontology stability
- architectural memory
- operational discipline
- bounded AI behavior

TradeForge development is itself treated as a replayable cognition system.

---

# Core Principle

TradeForge development is NOT:

- ad hoc coding
- implementation-first engineering
- uncontrolled feature expansion
- generic CRUD application development

TradeForge development IS:

- architecture-first
- workflow-centric
- replay-oriented
- event-aware
- semantically governed
- progressively stabilized

The workflow exists to ensure:
- implementation remains reconstructable
- architecture remains explainable
- ontology remains stable
- future agents retain semantic continuity

---

# Workflow Operationalization

The Runtime ↔ KB Development Loop defines:
- governance
- stabilization rules
- operational doctrine
- replayability discipline
- semantic consistency requirements

Execution behavior is operationalized through workflow prompts located in:

```text
prompts/workflow/
```

Examples:

```text
prompts/workflow/runtime-planning.md
prompts/workflow/runtime-implementation.md
prompts/workflow/kb-processing.md
prompts/workflow/operational-sync.md
```

These prompts provide:
- reusable invocation surfaces
- reproducible Codex behavior
- semantic initialization guidance
- replayability enforcement
- synchronization enforcement
- operational discipline

The playbook defines:
- WHY the workflow exists
- WHAT rules govern it

Workflow prompts define:
- HOW Codex operationalizes those rules during execution

---

# Required Context Initialization

Before planning or implementation work begins, semantic context MUST be initialized.

Required KB documents:

- [[SEMANTIC_BOOTSTRAP]]
- [[INVARIANTS]]
- [[ARCHITECTURE]]
- [[UX_DOCTRINE]]
- [[GLOSSARY]]
- [[EVENT_TAXONOMY]]
- [[EXECUTION_CONTRACT]]

Optional depending on task:

- [[PERSONAS]]
- [[WORKSPACES]]

Required runtime documents:

- `TradeForge/DOCS/`
- `TradeForge/DOCS/adr/`
- `TradeForge/DOCS/MILESTONE_ROADMAP.md`
- `TradeForge/DOCS/ISSUE_REGISTER.md`

If semantic initialization has not occurred:
STOP.

---

# Repository Roles

## Runtime Repository

Purpose:
- executable implementation
- lifecycle engine
- event systems
- replay systems
- integrations
- projections
- runtime workflows

Canonical locations:
- `DOCS/`
- `DOCS/adr/`
- `src/`
- `tests/`

---

## Knowledge Base Repository

Purpose:
- semantic governance
- ontology stabilization
- replayable reasoning
- workflow cognition
- architectural memory
- operational doctrine

Canonical locations:
- `playbooks/`
- `workflows/`
- `ontology/`
- `skills/`
- `prompts/`
- `knowledge/raw/`
- `knowledge/processed/`
- `knowledge/topics/`
- `knowledge/entities/`

---

# KB Knowledge Stabilization Model

TradeForge follows progressive semantic refinement.

Knowledge evolves through:

```text
raw discovery
    ↓
processed synthesis
    ↓
topic refinement
    ↓
canonical entity definition
    ↓
architectural stabilization
```

---

# Knowledge Layer Definitions

## knowledge/raw/

Contains:
- brainstorming
- issue planning captures
- implementation captures
- exploratory architecture
- unstable ideas
- temporary synthesis

Rules:
- NOT canonical
- disposable
- may contain noise
- preserves reasoning before stabilization

---

## knowledge/processed/

Contains:
- refined synthesis
- normalized terminology
- extracted architectural observations
- cleaned implementation learnings

Rules:
- semi-stable
- interpreted
- candidate for promotion

---

## knowledge/topics/

Contains:
- recurring architectural themes
- thematic syntheses
- operational concepts

Examples:
- replayability
- semantic stabilization
- bounded AI
- deterministic authority

---

## knowledge/entities/

Contains:
- canonical semantic concepts
- stable ontology-bearing objects

Examples:
- [[TradePlan]]
- [[Replay Session]]
- [[Workspace State]]
- [[Rule Evaluation]]

---

## playbooks/

Contains:
- operational governance systems
- repeatable development workflows
- cognition discipline

---

## workflows/

Contains:
- executable reasoning flows
- structured operational sequences

---

## skills/

Contains:
- reusable reasoning capabilities
- modular cognition patterns

Boundary:
- skills define how Codex should think
- skills preserve reusable reasoning constraints
- skills should not become procedural task entrypoints

---

## prompts/

Contains:
- reusable invocation surfaces
- restart contexts
- workflow prompts
- operational orchestration prompts

Examples:
- `prompts/workflow/runtime-planning.md`
- `prompts/workflow/runtime-implementation.md`
- `prompts/workflow/kb-processing.md`
- `prompts/workflow/operational-sync.md`

These prompts operationalize the Runtime ↔ KB Development Loop and provide reproducible execution behavior for Codex sessions.

Boundary:
- prompts define what Codex should do
- prompts preserve reusable operational workflows
- prompts may invoke skills, but should not absorb skill responsibilities

Durable distinction:

```text
skills  = reusable thinking constraints
prompts = reusable operational workflows
```

Related ontology:

- [[CodexSkill]]
- [[WorkflowPrompt]]
- [[Codex Operating Boundaries]]

---

# Operational Modes

TradeForge development operates in two modes.

---

# 1. Runtime Mode

Purpose:
- implementation
- testing
- issue execution
- runtime architecture

Focus:
- bounded implementation
- lifecycle integrity
- replay-safe design

---

# 2. KB Mode

Purpose:
- semantic stabilization
- ontology refinement
- doctrine preservation
- workflow governance
- replayable project cognition

Focus:
- semantic consistency
- architectural coherence
- stabilization of knowledge

---

# Canonical Development Loop

---

# Phase 1 — Select Work

In Runtime Mode:

1. Identify current milestone
2. Review:
   - `MILESTONE_ROADMAP.md`
   - `ISSUE_REGISTER.md`
   - relevant ADRs
   - relevant KB doctrine
3. Select next issue
4. Create or switch to issue branch if needed

Rules:
- no implementation without explicit issue scope
- milestone alignment is mandatory
- implementation must remain bounded

---

# Phase 2 — Context Initialization

Load semantic context before implementation planning.

Required:
- invariants
- glossary
- architecture doctrine
- event taxonomy
- execution contract
- relevant skills

Rules:
- no code generation before semantic initialization
- architecture interpretation precedes implementation
- ontology consistency is mandatory

---

# Phase 3 — Planning Before Implementation

Recommended prompt:

```text
prompts/workflow/runtime-planning.md
```

Enter planning mode before coding.

Planning must include:
- implementation approach
- affected modules
- affected entities
- event implications
- replay implications
- lifecycle implications
- testing strategy
- documentation impact
- ADR impact

Planning must follow the execution contract:

```text
1. Context Load
2. Skill Activation
3. Architectural Interpretation
4. Design Plan Generation
5. Validation Against Invariants
6. Code Output
```

Rules:
- architecture before implementation
- explicit reasoning preferred over shortcuts
- avoid premature abstraction
- code generation without planning is prohibited

---

# Phase 4 — ADR Checkpoint

Before implementation, verify whether an ADR is required.

Create or update an ADR if work introduces:

- architectural tradeoffs
- invariant changes
- lifecycle semantic changes
- new bounded contexts
- new replay behavior
- ontology-impacting structures
- event model changes
- projection model changes
- integration boundary changes
- durable future-facing decisions

Rules:
- runtime ADRs are canonical implementation authority
- missing ADRs create architectural drift
- KB may reference ADRs but does not replace them

Canonical location:

```text
TradeForge/DOCS/adr/
```

---

# Phase 5 — Capture Planning Into KB

Capture planning output into:

```text
knowledge/raw/
```

Suggested naming:

```text
YYYYMMDD-topic.md
```

Examples:

```text
20260508-position-replay-planning.md
20260508-rule-engine-refactor.md
```

Planning captures may include:
- issue goals
- architecture reasoning
- tradeoffs
- replay considerations
- unresolved questions
- implementation boundaries

Rules:
- raw captures are intentionally non-canonical
- preserve reasoning context
- avoid premature refinement

---

# Phase 6 — Process Planning Knowledge

Recommended prompt:

```text
prompts/workflow/kb-processing.md
```

Switch to KB Mode.

Process raw planning capture.

Possible actions:
- promote to processed knowledge
- refine terminology
- update topics
- update entities
- update ontology
- update workflows
- update playbooks
- update prompts
- preserve reusable reasoning
- discard unstable concepts

Rules:
- KB is an evolving semantic system
- processed knowledge MUST update canonical structures when justified
- semantic stabilization is mandatory

---

# Phase 7 — Implement

Recommended prompt:

```text
prompts/workflow/runtime-implementation.md
```

Return to Runtime Mode.

Implement ONLY the scoped issue.

Required activities:
- implementation
- testing
- replay validation
- lifecycle integrity validation
- event integrity validation

Rules:
- preserve bounded scope
- avoid unrelated features
- avoid ontology drift
- deterministic authority remains canonical

---

# Phase 8 — Capture Implementation Knowledge

After implementation:

Capture implementation results into:

```text
knowledge/raw/
```

Suggested contents:
- implementation summary
- tests executed
- replay implications
- architecture decisions
- unresolved concerns
- operational lessons
- follow-up opportunities

Rules:
- implementation knowledge is replayable cognition
- preserve architectural reasoning
- preserve operational learnings

---

# Phase 9 — Process Implementation Knowledge

Recommended prompt:

```text
prompts/workflow/kb-processing.md
```

Switch to KB Mode.

Process implementation capture.

Possible actions:
- promote durable knowledge
- stabilize terminology
- update doctrine
- update workflows
- update playbooks
- update ontology references
- update prompts
- update skills

Rules:
- implementation learnings are incomplete until stabilized
- semantic drift must be corrected immediately

---

# Phase 10 — Operational State Synchronization

Recommended prompt:

```text
prompts/workflow/operational-sync.md
```

Implementation is NOT complete until operational state is synchronized.

Mandatory updates:

- `MILESTONE_ROADMAP.md`
- `ISSUE_REGISTER.md`

Required checks:
- completed issues marked correctly
- milestone state accurate
- roadmap reflects implementation reality
- deferred work explicitly documented
- implementation state synchronized with planning state

Rules:
- stale operational state is architectural drift
- roadmap inconsistency is replay failure
- operational documents are authoritative planning systems

---

# Phase 11 — Commit Runtime Repository

Review runtime repository state.

Commit:
- implementation
- tests
- ADRs
- roadmap updates
- issue register updates
- runtime documentation

Rules:
- commits should represent coherent architectural state
- partial semantic states should be avoided

---

# Phase 12 — Commit KB Repository

Review KB repository state.

Commit:
- processed knowledge
- stabilized doctrine
- playbook updates
- ontology refinements
- workflow updates
- semantic promotions

Rules:
- KB commits preserve architectural memory
- semantic stabilization must remain replayable

---

# Milestone Transition Workflow

Milestone transitions are treated as architectural stabilization events.

These transitions are intentionally manual.

---

## Milestone Transition Sequence

1. Complete milestone implementation
2. Synchronize operational state
3. Process remaining KB captures
4. Verify ADR completeness
5. Push repositories to origin
6. Merge milestone branch into main
7. Verify stabilized main state
8. Create next milestone branch
9. Resume canonical development loop

---

## Milestone Transition Rules

Milestone transitions are:
- semantic boundaries
- architectural checkpoints
- replay stabilization points

Avoid:
- automated merge pipelines
- hidden milestone drift
- incomplete stabilization before merge

Correct architectural stabilization is more important than automation speed.

---

# Final Checklist

Before considering work complete:

- [ ] Semantic initialization completed
- [ ] Issue remained within scope
- [ ] Architecture reviewed before implementation
- [ ] Tests and validation executed
- [ ] Replayability preserved
- [ ] Lifecycle integrity preserved
- [ ] Event integrity preserved
- [ ] ADR need evaluated
- [ ] ADR created or updated if required
- [ ] Planning capture created
- [ ] Planning knowledge processed
- [ ] Implementation capture created
- [ ] Implementation knowledge processed
- [ ] MILESTONE_ROADMAP updated
- [ ] ISSUE_REGISTER updated
- [ ] Runtime repository committed
- [ ] KB repository committed
- [ ] Milestone state synchronized
- [ ] Semantic consistency verified

---

# Anti-Patterns

Avoid:
- dashboard creep
- ontology explosion
- uncontrolled AI additions
- implementation without planning
- skipping semantic initialization
- skipping ADR evaluation
- stale roadmap state
- stale issue register state
- semantic drift
- undocumented replay assumptions
- hidden lifecycle behavior
- premature automation
- feature sprawl

---

# Closing Principle

TradeForge development is itself treated as a replayable cognition system.

The workflow exists to ensure:
- architecture remains reconstructable
- implementation remains explainable
- ontology remains stable
- operational memory persists
- future agents retain semantic continuity

Correctness, replayability, and semantic integrity remain more important than implementation velocity.
