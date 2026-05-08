# Skill: Workspace Querying

## Purpose

Defines how Codex should interpret and reason about workspace state.

---

# Core Principle

A workspace is:
> a persistent decision environment, not a UI container

---

# Query Rule

When reasoning about a workspace:

Codex must distinguish:

- canonical state (event-derived)
- derived projections (UI/state views)
- inferred insights (AI interpretation)

Never mix these layers.

---

# Workspace Dimensions

All workspace queries must consider:

## 1. Exposure State
- what positions exist
- risk distribution
- current commitments

## 2. Decision State
- pending decisions
- lifecycle stage of ideas
- approval queue

## 3. Scenario State
- active scenarios
- ranked opportunities
- invalidated hypotheses

## 4. Context State
- market regime
- macro conditions
- persona context

---

# Hard Constraint

Codex MUST NOT:

- treat workspace as a static snapshot
- assume UI equals state
- merge derived and canonical data

---

# Correct Reasoning Pattern

> “This workspace reflects derived exposure state reconstructed from events”

NOT:

> “The workspace currently holds these positions internally”

---

# Final Principle

Workspaces are reconstructed cognition, not stored dashboards.