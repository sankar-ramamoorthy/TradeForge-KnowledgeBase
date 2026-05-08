---
title: MVP Evolution v1-v3
type: brainstorm-synthesis
status: processed
tags: [TradeForge, brainstorm, MVP, roadmap, replayable-cognition]
created: 2026-05-08
updated: 2026-05-08
source_notes:
  - knowledge/raw/brainstorm-mvp-v1-replayable-workflow-system.md
  - knowledge/raw/brainstorm-mvp-v2-contextual-cognition.md
  - knowledge/raw/brainstorm-mvp-v3-adaptive-scenario-intelligence.md
---

# MVP Evolution v1-v3

## Status

Processed brainstorm. This is a directional product/architecture synthesis, not canonical roadmap authority.

## North Star

TradeForge should prove replayable cognition, not trading alpha, autonomous execution, dashboard sophistication, or AI hype.

The long-term sequence is:

```text
v1: replayable workflow system
v2: contextual cognition system
v3: adaptive scenario-driven decision platform
```

The enduring question at every stage:

```text
Does this preserve replayable cognition?
```

## MVP v1: Replayable Workflow System

### Goal

Prove that TradeForge can model, execute, replay, and review a disciplined discretionary trading workflow using event-sourced cognition.

The smallest coherent slice:

- one trader
- one workflow
- one lifecycle
- one projection model
- one replay model
- one review loop

### Core Narrative

```text
1. Trader creates an idea
2. Trader forms a thesis
3. Trader builds a plan
4. Rules validate the plan
5. Trader executes manually
6. Fills are recorded
7. Position state is reconstructed from events
8. Trade is closed
9. Review is performed
10. Replay shows intent, outcome, violations, and lessons
```

### Mandatory Capabilities

- Event sourcing
- Canonical lifecycle: `Idea -> Thesis -> Plan -> Approval -> Execution -> Position -> Review`
- Deterministic rule evaluation
- Replay engine
- Simple workspace projection layer

### Explicit Deferrals

- No RL
- No autonomous trading
- No multi-agent systems
- No advanced AI orchestration
- No vector database
- No massive UI
- No real-time streaming complexity
- No large market data infrastructure

### MVP v1 Workspaces

- Operating Workspace
- Plan Review Workspace
- Active Position Workspace
- Replay Workspace
- Review Workspace

### Technical Shape

- Python 3.12
- JSON event ledger initially
- Simple projection builders for positions, pending reviews, active plans, and violations
- TUI is a strong candidate because it can feel operational, avoid dashboard drift, and focus cognition

### Success Signal

The MVP succeeds if it reads as:

```text
an event-sourced decision operating system for discretionary trading
```

## MVP v2: Contextual Cognition System

### Goal

Prove that TradeForge can begin understanding, contextualizing, and improving trading behavior over time.

v2 should introduce bounded intelligence into a stable deterministic system, not add AI everywhere.

### Core Theme

```text
context-aware advisory cognition
```

### Candidate Capabilities

- Market Context Engine
- Pattern Recognition Layer
- Behavioral Intelligence
- Early Scenario Engine
- Intelligence Workspaces
- AI-assisted trade review
- Knowledge Base integration

### Market Context Engine

The system should interpret volatility regime, trend regime, sector leadership, macro context, earnings environment, liquidity conditions, and risk-on/risk-off state.

This remains contextual interpretation, not prediction and not lifecycle authority.

### Behavioral Intelligence

TradeForge can begin detecting repeated rule violations, emotional overrides, sizing drift, revenge trading, hesitation behavior, stop discipline decay, and thesis inconsistency.

### Contextual Replay Intelligence

Signature v2 query:

```text
Show me all failed breakout trades
during high volatility
where I violated stop discipline
and compare them against successful versions.
```

### Continued Deferrals

- Still no full RL
- Still no autonomous execution
- No "AI trader"
- Avoid feature explosion, generic analytics, dashboard creep, and uncontrolled AI additions

## MVP v3: Adaptive Scenario-Driven Decision Platform

### Goal

Prove adaptive scenario-driven decision augmentation.

The system begins helping answer:

```text
What situation am I actually in,
what usually happens here,
and which playbook applies?
```

### Signature Capability

Scenario Intelligence Engine.

This is not backtesting, signal generation, generic AI, or prediction. It is scenario cognition.

### Candidate Components

- Scenario Graph Engine
- Playbook Intelligence
- Simulation and Counterfactual Replay
- Decision Quality Scoring
- Adaptive Workspace Generation
- Knowledge Graph / Semantic Memory Layer
- Carefully bounded early multi-agent advisory
- Narrow RL exploration for scenario-policy learning and playbook adaptation

### Decision Quality Scoring

Decision quality should not reduce to PnL. Candidate dimensions:

- plan completeness
- rule adherence
- thesis consistency
- emotional stability
- execution discipline
- regime alignment
- review quality

### Adaptive Workspace Generation

Workspaces should dynamically emphasize relevant playbooks, current risks, regime warnings, behavioral drift, pending reviews, and historical analogs.

### Critical Risk

Ontology explosion.

v3 introduces regimes, playbooks, scenarios, advisors, behavioral models, semantic relationships, and replay abstractions. Without governance, the system becomes incoherent.

## Architectural Throughline

Across all versions:

- Event Ledger remains canonical.
- Decision Lifecycle Engine remains authoritative.
- Replay remains foundational.
- AI remains advisory.
- Human decision sovereignty remains mandatory.
- Workspaces remain cognitive environments, not dashboards.
- The KB becomes increasingly important as semantic memory and governance infrastructure.

## Possible Next Outputs

- Product roadmap topic page
- MVP scope ADR
- v1 vertical slice implementation guide
- SwingTrader MVP workflow note
- Replayable cognition topic page
