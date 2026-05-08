# TradeForge — Workspaces

## Purpose

This document defines the canonical workspace model in TradeForge.

Workspaces are persona-scoped operational environments that structure:

- situational awareness
- decision workflows
- scenario evaluation
- exposure tracking
- review and reflection

Workspaces are not UI constructs.

They are **persistent cognitive and operational contexts**.

---

# Core Principle

## Workspaces Are Stateful Decision Environments

A workspace represents:

> a continuous decision context over time for a given persona

It maintains:
- active market interpretation
- open decision workflows
- exposure state
- opportunity space
- review history

Workspaces persist across sessions.

---

# Relationship to Personas

Workspaces are always bound to a persona:

```text
Persona → Workspace → Decision System
```

A workspace inherits:
- interpretation bias
- risk framing
- signal preferences
- decision velocity
- playbook relevance

A workspace is NOT usable without a persona context.

---

# Workspace Types

Workspaces are not generic. They have distinct operational roles.

---

## 1. Active Trading Workspace

Purpose:
- live decision-making
- active scenario evaluation
- position monitoring

Characteristics:
- high temporal sensitivity
- frequent updates
- active exposure tracking

Focus:
> what requires action now

---

## 2. Watchlist Workspace

Purpose:
- monitoring emerging opportunities
- scenario incubation
- pre-decision filtering

Characteristics:
- medium update frequency
- scenario accumulation
- low execution pressure

Focus:
> what might become actionable

---

## 3. Research Workspace

Purpose:
- deep analysis
- thesis development
- regime understanding

Characteristics:
- slow-moving
- high context depth
- analytical focus

Focus:
> why something behaves as it does

---

## 4. Review Workspace

Purpose:
- replay and reflection
- performance analysis
- behavioral learning

Characteristics:
- historical focus
- event-driven reconstruction
- non-execution environment

Focus:
> what happened and why

---

## 5. Risk & Exposure Workspace

Purpose:
- monitor portfolio risk
- evaluate systemic exposure
- track violations

Characteristics:
- constraint-driven
- alert-heavy
- enforcement-oriented

Focus:
> what risk is currently present

---

# Workspace Structure

Each workspace contains structured decision surfaces:

## 1. Briefing Surface
- current market context
- regime state
- relevant macro shifts

---

## 2. Opportunity Surface
- scenario candidates
- ranked opportunities
- watchlist inflows

---

## 3. Exposure Surface
- active positions
- unrealized outcomes
- thesis alignment status

---

## 4. Decision Queue
- required actions
- lifecycle transitions
- pending approvals

---

## 5. Review Surface
- replay sessions
- behavioral analysis
- rule evaluations

---

# Workspace State Model

Workspaces maintain **derived operational state**, not canonical truth.

Workspace state is built from:

- Event Ledger
- Decision Lifecycle Engine
- Market Intelligence Layer
- Scenario Engine outputs

Workspaces are projections, not sources of truth.

---

# Workspace Lifecycle

Workspaces have lifecycle states:

## Active
- currently in use
- real-time updates
- full system integration

## Dormant
- paused context
- state preserved
- no active updates

## Archived
- historical snapshot
- replay-only mode
- immutable reference

---

# Workspace Activation Rule

Only one Active Workspace per persona context should be dominant at a time.

Multiple workspaces may exist, but:
- one is primary for decision flow
- others are secondary or monitoring contexts

---

# Workspace vs UI

Workspaces are NOT UI tabs.

UI is a projection layer over workspace state.

Incorrect model:
- “open tab = workspace”

Correct model:
- workspace = persistent decision environment
- UI = window into that environment

---

# Workspace Data Inputs

Workspaces consume:

- Market Intelligence Layer outputs
- Scenario Discovery outputs
- Event Ledger state
- Decision Lifecycle Engine state
- Execution feedback

They do NOT directly consume raw external APIs.

---

# Workspace Outputs

Workspaces produce:

- decision queue state
- opportunity prioritization
- exposure awareness
- review artifacts
- user-facing cognitive surfaces

---

# Workspace Isolation Principle

Workspaces must maintain logical separation:

- no cross-workspace state leakage
- no implicit shared decision state
- no hidden coupling between personas/workspaces

Each workspace is self-contained in interpretation.

---

# Cognitive Continuity Principle

Workspaces must preserve:

- decision context over time
- reasoning continuity
- scenario evolution
- exposure evolution

Users must never “lose the thread” when returning to a workspace.

---

# Workspace and Replay

Workspaces are fully replayable via:

- Event Ledger
- Deterministic state reconstruction
- Historical snapshots

Replay reconstructs:
- briefing state
- opportunity surface
- exposure state
- decision queue
- review state

---

# Workspace Design Principle

Workspaces are designed to answer:

1. What is happening?
2. What matters now?
3. What can I do about it?
4. What am I exposed to?
5. What did I learn from this?

---

# Invalid Workspace Patterns

The following are NOT valid workspace concepts:

- “dashboard workspace”
- “UI workspace”
- “settings workspace”
- “data view workspace”

Reason:
These describe interfaces, not decision environments.

---

# AI Role in Workspaces

AI may:

- enhance briefing summaries
- cluster scenarios
- highlight risk changes
- assist review reconstruction

AI may NOT:

- define workspace state
- override decision queues
- mutate lifecycle state directly

AI is a contextual layer inside the workspace, not the workspace owner.

---

# Final Principle

Workspaces are where cognition becomes structured.

If workspaces are weak:

- decision context fragments
- scenario relevance degrades
- exposure awareness breaks
- replay becomes incoherent

Therefore:

> workspace design is decision-system design