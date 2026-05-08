# TradeForge — UX Doctrine

## Purpose

This document defines the interaction philosophy, cognitive model, and decision-support UX principles for TradeForge.

It is not a UI specification.

It defines how the system should present information to support:

- situational awareness
- decision-making under uncertainty
- workflow continuity
- reflection and learning
- reduced cognitive fragmentation

All runtime UI implementations must conform to these principles.

---

# Core UX Philosophy

TradeForge UX is:

- workflow-centric
- decision-centric
- context-first
- persona-driven
- reflection-aware
- cognitive-load optimized

TradeForge UX is NOT:

- dashboard-centric
- navigation-heavy
- data-dense by default
- feature-centric
- UI-component-driven

The interface exists to support thinking and decisions, not to display data.

---

# Primary UX Principle

## Context Before Action

The system must always establish:

1. What is happening?
2. Why does it matter?
3. What is the user responsible for?
4. What decisions are pending?

Only then should actions be presented.

Never present actions without context.

---

# Persona-Driven Interaction Model

All UX surfaces are scoped by persona.

Each persona defines:

- information priorities
- risk sensitivity
- time horizon
- playbook relevance
- decision cadence

The same data may be presented differently depending on persona context.

UX is therefore not universal — it is **adaptive cognition**.

---

# Workspace Model

The primary UX unit is the **Persona Workspace**.

A workspace is an operational environment containing:

- situational awareness
- active exposure
- opportunity discovery
- decision queue
- review surface

Workspaces are NOT navigation containers.

They are:
> structured decision environments

---

# Decision Surfaces

TradeForge UX is composed of decision surfaces, not pages.

## 1. Briefing Surface

Purpose:
- establish situational awareness

Contains:
- market regime summary
- macro changes
- key events
- exposure summary
- risk signals

Goal:
> orient the user

---

## 2. Opportunity Surface

Purpose:
- surface candidate actions

Contains:
- scenario candidates
- ranked opportunities
- watchlist changes
- emerging setups

Goal:
> focus attention

---

## 3. Exposure Surface

Purpose:
- show current commitments

Contains:
- positions
- risk exposure
- unrealized outcomes
- thesis state

Goal:
> maintain awareness of responsibility

---

## 4. Decision Queue

Purpose:
- surface required actions

Contains:
- approvals needed
- lifecycle transitions required
- stale theses
- risk violations

Goal:
> reduce ambiguity in next steps

---

## 5. Review Surface

Purpose:
- enable reflection and learning

Contains:
- replay sessions
- outcomes vs expectations
- rule evaluations
- mistakes and deviations
- behavioral insights

Goal:
> improve future decision quality

---

# Cognitive Load Principle

The system must actively manage cognitive load.

Rules:

- do not overload a single surface with unrelated signals
- prioritize relevance over completeness
- hide noise unless explicitly requested
- cluster related signals into interpretable units

Information density must be:
> adaptive, not maximal

---

# Hierarchy of Attention

UX must enforce information priority:

1. Immediate decisions required
2. Active risk exposure
3. Emerging opportunities
4. Contextual background
5. Historical data

Lower priority information must not compete visually with higher priority signals.

---

# Workflow Continuity Principle

UX must preserve continuity of thought.

Rules:

- users should not lose context when moving between surfaces
- decisions must remain traceable across views
- transitions between surfaces must preserve state awareness
- no “context resets” between UI sections

The system behaves as a continuous cognitive environment.

---

# Uncertainty Must Be Visible

TradeForge does NOT hide uncertainty.

UX must explicitly surface:

- incomplete information
- conflicting signals
- low-confidence scenarios
- ambiguous regimes

Uncertainty is part of the decision context, not noise to be removed.

---

# Reflection Is a First-Class UX Element

Review is not a secondary analytics feature.

It is a core interaction mode.

UX must support:

- replaying decisions
- comparing intent vs outcome
- identifying rule violations
- surfacing behavioral patterns

Learning is a continuous loop, not a separate module.

---

# Navigation Principle

Navigation is secondary to cognition.

Users should not “browse” the system.

Instead they should:
- enter a workspace
- understand context
- identify decisions
- execute workflows
- reflect on outcomes

Navigation exists only to support transitions between cognitive states.

---

# AI in UX

AI is embedded in UX as a contextual assistant.

AI may:

- summarize context
- highlight anomalies
- rank opportunities
- explain scenarios
- assist review

AI may NOT:

- override workflow state
- suppress uncertainty
- replace decision surfaces
- act as authoritative controller

AI enhances perception, not authority.

---

# Information Presentation Principle

Information must be:

- structured
- interpretable
- grouped by relevance
- context-aware

Avoid:

- raw data dumps
- unstructured feeds
- excessive simultaneous signals
- non-prioritized lists

The goal is:
> interpretability, not volume

---

# UI as Cognitive System

TradeForge UI is not a visualization layer.

It is a cognitive system that supports:

- attention allocation
- decision formation
- risk awareness
- reflection
- adaptation

Every UI element must justify its existence in cognitive terms.

---

# Final Principle

TradeForge UX exists to improve:

- clarity of thought
- quality of decisions
- awareness of risk
- continuity of workflow
- depth of reflection

The system succeeds only if it improves human judgment under uncertainty.