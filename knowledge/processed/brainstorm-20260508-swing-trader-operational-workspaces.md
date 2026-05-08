---
title: Swing Trader Operational Workspaces
type: brainstorm-synthesis
status: processed
tags: [TradeForge, brainstorm, swing-trader, operational-workspaces, persona-workspace]
created: 2026-05-08
updated: 2026-05-08
source_notes:
  - knowledge/raw/Brainstorm session on operation flow for sqing trader persona.md
  - knowledge/raw/brainstorm-20260508-swing-trader-operational-workspaces.md
---

# Swing Trader Operational Workspaces

## Status

Processed brainstorm. This is exploratory cognition, not canonical architecture.

## Core Thought

The Swing Trader experience should be modeled as a set of operational cognitive environments, not as dashboard screens or navigation tabs.

TradeForge should mirror the swing trader operating rhythm:

```text
Orient
Assess exposure
Discover opportunities
Evaluate setups
Build plans
Execute/manage
Review
```

The key design move is to optimize for decision quality and workflow continuity rather than information density.

## Candidate Workspaces

### Morning Briefing Workspace

Purpose:

```text
What environment am I operating in today?
```

This is the situational awareness workspace. It should emphasize regime awareness, volatility state, sector rotation, macro events, earnings/events relevant to held positions, overnight changes, open decision queues, and active thesis risk alerts.

### Active Exposure Workspace

Purpose:

```text
What am I currently exposed to?
```

This is the risk cognition workspace. It should represent thesis exposure, sector concentration, regime sensitivity, correlated risk, unrealized lifecycle state, stop integrity, violated assumptions, earnings exposure, sizing anomalies, and rule violations.

### Opportunity Discovery Workspace

Purpose:

```text
What deserves attention?
```

This is the Scenario Discovery surface for the Swing Trader persona. It is not a screener. It should explain why something surfaced, which persona it aligns with, which playbook it resembles, and which conditions make it operationally relevant.

### Decision Queue Workspace

Purpose:

```text
What requires a decision from me?
```

This workspace should surface plans awaiting approval, stop adjustments requiring review, thesis invalidation events, scaling decisions, earnings proximity decisions, exit decisions, replay/review tasks, and unresolved anomalies.

### Thesis / Plan Workspace

Purpose:

```text
Think deeply before committing capital.
```

This is where a swing trade is formed through [[TradeIdea]], [[TradeThesis]], and [[TradePlan]]. It should preserve scenario assumptions, invalidation conditions, catalysts, risk model, playbook alignment, entry/exit structure, sizing rationale, and expected holding horizon.

### Replay And Review Workspace

Purpose:

```text
What actually happened?
```

This is reflective cognition infrastructure, not performance reporting. It should support replay sessions, historical context reconstruction, decision timelines, thesis evolution, missed signals, violated rules, market regime comparison, post-trade annotation, and bounded AI-assisted pattern surfacing.

### Market Intelligence Workspace

Purpose:

```text
What is the market actually doing?
```

This is interpreted context, not raw charts. It should contain regime analysis, breadth, liquidity conditions, volatility structure, sector leadership, macro interpretation, earnings climate, correlation shifts, and narrative shifts.

## Operational Flow

The normal Swing Trader daily flow should resemble:

```text
Morning Briefing
    ->
Active Exposure Review
    ->
Opportunity Discovery
    ->
Decision Queue
    ->
Thesis / Plan Construction
    ->
Execution Management
    ->
Replay & Review
```

## Design Implications

- The Swing Trader workspace should not become a generic watchlist, chart wall, position table, or KPI dashboard.
- Each workspace should be a projection of event ledger facts, lifecycle state, scenario state, market intelligence, and persona context.
- The system is projecting cognition state, workflow state, and operational state into coherent environments.
- SwingTrader may become a canonical [[Persona]], a workspace configuration, or both, but that decision should happen in a later canonicalization pass.

## Open Questions

- Is SwingTrader a canonical persona, a workspace variant, or an operational mode within a broader trader persona?
- What time horizon and review cadence define SwingTrader?
- Which signals are operationally relevant for swing trading versus noise?
- Which decisions should the workspace prioritize each day?

## Concerns

- Avoid dashboard drift.
- Avoid encoding strategy rules before persona/workspace semantics are clear.
- Keep AI advisory only.
- Preserve replay requirements for decisions made inside the workspace.

## Possible Next Outputs

- Persona candidate: SwingTrader
- Workspace topic page
- Workflow refinement
- Entity relationship note
- UX doctrine extension
