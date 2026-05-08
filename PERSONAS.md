# TradeForge — Personas

## Purpose

This document defines the canonical persona model used in TradeForge.

Personas define how the system:
- interprets market context
- prioritizes signals
- ranks scenarios
- structures decision workflows
- shapes workspace behavior

Personas are a core part of the system architecture, not a UX feature.

---

# Core Principle

## Personas Are Behavioral Systems

A persona is not a user type.

A persona is a **decision lens** that defines:
- how information is filtered
- how uncertainty is handled
- how opportunities are prioritized
- how risk is interpreted

Personas shape cognition, not identity.

---

# System Role of Personas

Personas sit at the top of the architecture:

```text
Persona
    ↓
Persona Workspace
    ↓
Market Interpretation
    ↓
Scenario Ranking
    ↓
Decision Lifecycle
```

Everything downstream is conditioned by persona context.

---

# Persona Properties

Each persona defines:

## 1. Time Horizon Bias
- intraday
- swing
- positional
- long-term

This affects:
- signal sensitivity
- scenario weighting
- risk tolerance framing

---

## 2. Risk Framing Model
Defines how risk is interpreted:

- capital preservation oriented
- balanced
- aggressive
- asymmetric risk seeking

Risk framing influences:
- position sizing logic
- scenario rejection thresholds
- invalidation sensitivity

---

## 3. Decision Velocity
Defines how quickly decisions are made:

- reactive
- balanced
- deliberate
- highly selective

This affects:
- workflow urgency
- queue prioritization
- approval strictness

---

## 4. Signal Preference Bias
Defines what types of signals are prioritized:

- technical
- macro
- sentiment
- order flow
- multi-factor hybrid

This shapes:
- Market Intelligence weighting
- Scenario generation emphasis

---

## 5. Playbook Alignment
Each persona is associated with a set of playbooks.

Playbooks define:
- preferred setups
- avoided conditions
- historical edge patterns

---

# Persona Types (Canonical Examples)

These are reference personas, not exhaustive implementations.

---

## 1. Swing Operator Persona

### Characteristics:
- medium time horizon
- structured but flexible decision process
- moderate risk tolerance
- prefers multi-factor confirmation

### Behavior:
- waits for alignment across signals
- prioritizes scenario quality over quantity
- emphasizes structured thesis formation

### Workspace Impact:
- slower decision queues
- deeper review emphasis
- fewer but higher quality scenarios

---

## 2. Tactical Trader Persona

### Characteristics:
- short time horizon
- high responsiveness
- execution-focused
- higher signal sensitivity

### Behavior:
- reacts quickly to regime shifts
- prioritizes timing over narrative depth
- accepts higher uncertainty

### Workspace Impact:
- fast-moving decision queue
- frequent scenario updates
- high alert sensitivity

---

## 3. Macro Allocator Persona

### Characteristics:
- long time horizon
- macro-driven interpretation model
- low frequency decisions
- high conviction thresholds

### Behavior:
- prioritizes regime shifts
- ignores short-term noise
- focuses on structural opportunities

### Workspace Impact:
- slow update cadence
- emphasis on market intelligence layer
- fewer scenarios, higher abstraction

---

## 4. Risk-Controlled Operator Persona

### Characteristics:
- capital preservation priority
- strict rule adherence
- conservative execution profile

### Behavior:
- rejects marginal setups
- enforces strict invalidation rules
- prioritizes downside protection

### Workspace Impact:
- high rejection rate for scenarios
- strict lifecycle gating
- strong review enforcement

---

# Persona-Driven System Behavior

All major system components must adapt to persona context:

## Market Intelligence Layer
- changes weighting of signals
- adjusts interpretation framing
- filters noise differently

---

## Scenario Discovery Engine
- alters ranking strategy
- changes threshold for inclusion
- modifies clustering behavior

---

## Decision Lifecycle Engine
- adjusts approval strictness
- modifies workflow sensitivity
- changes queue prioritization logic

---

## UX Layer
- adapts information density
- adjusts default surface emphasis
- modifies alerting behavior

---

# Persona Consistency Rule

A persona must remain consistent across:

- workspace sessions
- decision cycles
- replay sessions
- review analysis

Personas are NOT dynamically changing per screen or interaction.

---

# Invalid Persona Patterns

The following are NOT valid personas:

- “Beginner trader”
- “Advanced user”
- “Bullish trader”
- “Crypto trader”
- “Aggressive mode”

Reason:
These describe states or preferences, not stable decision systems.

---

# Persona vs User

| Concept | Meaning |
|--------|--------|
| User | identity/account |
| Persona | decision behavior model |

A single user may operate multiple personas.

A persona is selected per workspace context.

---

# Persona Activation Rule

A persona must be explicitly activated in a workspace.

Activation defines:
- interpretation mode
- scenario weighting model
- workflow behavior profile

No implicit persona switching is allowed during active workflows.

---

# Persona Influence Scope

Personas influence:

- interpretation of market data
- scenario generation bias
- decision workflow strictness
- risk evaluation framing
- review interpretation lens

Personas do NOT:
- execute trades
- own event authority
- override lifecycle rules

---

# Persona Evolution Rule

Personas may evolve, but:

- changes must be versioned
- historical replay must preserve original persona state
- prior decision context must remain reconstructable

Personas are not mutable in hindsight.

---

# Design Principle

Personas exist to encode:
> how decisions are made, not who is making them

They are structural cognitive models, not UI preferences.

---

# Final Principle

If personas are weak or ambiguous:

- scenario ranking becomes unstable
- UX becomes generic
- decision quality degrades
- replay loses interpretability

Therefore:

> persona design is system design