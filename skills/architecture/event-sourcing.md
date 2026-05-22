---
id: skill-event-sourcing-discipline
title: Event Sourcing Discipline
category: architecture
applies_to:
  - codex
  - claude
version: 1.0
description: Enforces strict event-sourced architecture — all state derives from events, replayability is mandatory, projections are never canonical truth.
---

# Skill: Event Sourcing Discipline

## Purpose

Ensure strict adherence to event-sourced architecture within TradeForge.

This skill governs all state-related reasoning.

---

# Core Rule

## Events Are The Only Source Of Truth

All system state must be derived from events.

No exceptions.

---

# Hard Constraints

Agent MUST NOT:

- mutate state directly
- assume current system state without events
- bypass lifecycle engine
- treat projections as canonical truth

---

# Allowed Actions

Agent MAY:

- define new event types
- extend event taxonomy
- design event flows
- reason about state derived from events

---

# State Interpretation Rule

If state is required:

1. derive from Event Ledger
2. reconstruct via deterministic rules
3. or explicitly mark as “derived projection”

Never assume hidden state.

---

# Replay Requirement

All event-driven systems must be:
- replayable
- deterministic
- reconstructable

If a design breaks replayability:
→ it is invalid

---

# Anti-Pattern

Invalid reasoning pattern:

> “The system likely knows current position state internally”

Correct pattern:

> “Position state must be reconstructed from event history”

---

# Final Principle

If it is not an event, it is not truth.