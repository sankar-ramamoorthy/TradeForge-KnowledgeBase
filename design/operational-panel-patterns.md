---
title: Operational Panel Patterns
status: draft
created: 2026-05-16
updated: 2026-05-16
type: design
domain: tradeforge
layer: operational-panel-architecture
tags:
  - tradeforge
  - design
  - workspace
  - panels
  - operational-cognition
  - ui
  - frontend
  - workstation
related:
  - "[[workspace-operational-layouts]]"
  - "[[workspace-zoning-reference]]"
  - "[[DESIGN_ARCHITECTURE]]"
  - "[[Operating Workspace]]"
  - "[[Replay Workspace]]"
  - "[[Review Workspace]]"
  - "[[Human Decision Sovereignty]]"
  - "[[AI Advisory Boundary]]"
runtime_related:
  - "TradeForge/frontend/DESIGN.md"
  - "TradeForge/frontend/src"
authority: design-doctrine
canonical: false
summary: >
  Defines reusable operational panel patterns for TradeForge workspaces,
  including authority boundaries, density expectations, panel behavior,
  contextual layering, and workstation-oriented operational cognition surfaces.
---

# Operational Panel Patterns

## Purpose

This document defines the reusable panel patterns used throughout TradeForge workspaces.

Panels are the primary operational surface primitive in TradeForge.

A panel is not merely:

```text
a visual card
````

A panel is:

```text
an operational cognition surface
```

Panels exist to support:

* parallel cognition
* rapid situational awareness
* workflow continuity
* structured operational monitoring
* contextual decision-making
* replay visibility
* authority separation

---

# Relationship To Other Design Documents

| Document                           | Responsibility                        |
| ---------------------------------- | ------------------------------------- |
| `DESIGN_ARCHITECTURE.md`           | Cognitive and operational philosophy  |
| `workspace-operational-layouts.md` | Overall workspace composition         |
| `workspace-zoning-reference.md`    | Spatial workspace regions             |
| `operational-panel-patterns.md`    | Reusable operational panel primitives |
| `frontend/DESIGN.md`               | Visual tokens and frontend primitives |

This document focuses specifically on:

```text id="j4wo14"
reusable operational surface structures
```

---

# Core Principle

TradeForge panels optimize for:

```text id="mjlwmz"
glanceable operational cognition
```

rather than:

```text id="rzw7b2"
decorative visual composition
```

Panels should allow the operator to:

* scan quickly
* compare rapidly
* inspect progressively
* maintain context
* preserve workflow continuity
* distinguish authority layers visually

---

# Panel Philosophy

TradeForge prefers:

```text id="6m7m1s"
many focused operational panels
```

over:

```text id="paj36p"
single monolithic operational pages
```

Panels should:

* compress useful operational state
* preserve semantic clarity
* minimize navigation cost
* support workstation density
* expose relevant actions without clutter

---

# Panel Characteristics

All operational panels should support some combination of:

| Capability             | Purpose                                         |
| ---------------------- | ----------------------------------------------- |
| Glanceability          | Rapid visual scanning                           |
| Progressive Disclosure | Expand deeper detail only when needed           |
| Status Visibility      | Current operational state visible immediately   |
| Authority Clarity      | Canonical vs advisory distinction               |
| Context Retention      | Preserve awareness during interaction           |
| Compact Density        | Efficient use of workspace                      |
| Action Proximity       | Relevant actions near relevant state            |
| Replayability          | Historical reasoning recoverable where relevant |

---

# Panel Taxonomy

TradeForge currently defines the following conceptual panel categories.

| Panel Type              | Purpose                         |
| ----------------------- | ------------------------------- |
| Decision Panel          | Active operational decision     |
| Position Panel          | Open exposure monitoring        |
| Plan Panel              | Structured trade intent         |
| Advisory Panel          | AI or contextual interpretation |
| Context Panel           | Market/environment awareness    |
| Replay Panel            | Historical reconstruction       |
| Review Panel            | Behavioral evaluation           |
| Queue Panel             | Attention triage                |
| Alert Panel             | Elevated operational attention  |
| Timeline Panel          | Temporal event navigation       |
| Rule Evaluation Panel   | Deterministic validation        |
| Workspace Summary Panel | Aggregated operational overview |

---

# 1. Decision Panel

## Purpose

The Decision Panel represents an active operational decision object.

It answers:

```text id="xxy4rr"
What is this opportunity and what is its current operational state?
```

---

## Typical Contents

* symbol
* setup classification
* thesis summary
* trigger condition
* armed state
* lifecycle stage
* invalidation
* risk context
* attention state
* next required action

---

## Typical Density

High.

Decision Panels should support rapid scanning across multiple active opportunities.

---

## Example Structure

```text id="wr65vn"
+--------------------------------------------------+
| NVDA                              Armed          |
| Momentum Breakout                                |
|                                                  |
| Trigger: > 232.50                                |
| Invalidation: < 224                              |
| Risk: Medium                                     |
| Earnings: 4 days                                 |
|                                                  |
| Thesis: AI acceleration continuation             |
|                                                  |
| [Review] [Disarm] [Replay]                       |
+--------------------------------------------------+
```

---

## Design Guidance

Decision Panels should:

* expose operational state immediately
* minimize hidden critical state
* support side-by-side comparison
* preserve thesis visibility
* avoid decorative dashboard styling

---

# 2. Position Panel

## Purpose

The Position Panel represents active exposure.

It answers:

```text id="j2okxm"
What risk is currently live?
```

---

## Typical Contents

* symbol
* quantity
* average basis
* unrealized state
* current thesis status
* stop/invalidation
* exposure classification
* position age
* drift indicators

---

## Example Structure

```text id="mxksga"
+--------------------------------------------------+
| SMH                              OPEN            |
| Qty: 40                                           |
| Avg: 242.10                                       |
| Current: 248.20                                   |
|                                                    |
| Stop: 236                                          |
| Target: 265                                        |
|                                                    |
| Thesis Drift: None                                 |
| Exposure: Moderate                                 |
|                                                    |
| [Review] [Adjust] [Replay]                         |
+--------------------------------------------------+
```

---

## Design Guidance

Position Panels should prioritize:

* exposure visibility
* thesis continuity
* invalidation clarity
* operational readiness
* behavioral awareness

Avoid:

* pure P/L dashboard mentality
* excessive visual noise
* detached metric grids

---

# 3. Plan Panel

## Purpose

The Plan Panel represents structured trade intent.

It answers:

```text id="6im4of"
What was intended before execution?
```

---

## Typical Contents

* setup
* thesis
* trigger
* entry assumptions
* invalidation
* sizing assumptions
* approval state
* rule evaluation summary
* arm status

---

## Design Guidance

Plan Panels should reinforce:

```text id="v0y1zg"
intent before execution
```

The operator should be able to reconstruct:

* why the plan exists
* what conditions matter
* what invalidates the setup

---

# 4. Advisory Panel

## Purpose

The Advisory Panel presents non-canonical interpretation.

It answers:

```text id="63ow45"
What interpretation or guidance should I consider?
```

---

## Authority Classification

Advisory Panels are:

```text id="6r2mgn"
non-authoritative
```

They must never visually resemble canonical operational truth.

---

## Typical Contents

Examples:

* AI summaries
* contextual briefings
* opportunity suggestions
* risk observations
* market interpretations
* comparative analysis
* pattern recognition

---

## Required Characteristics

Advisory Panels should support:

* provenance visibility
* timestamp visibility
* confidence/uncertainty where relevant
* explicit advisory framing

---

## Example Structure

```text id="g50w4l"
+--------------------------------------------------+
| AI Context Briefing                              |
| Advisory Only                                    |
|                                                   |
| Semiconductor momentum remains strong but         |
| breadth has narrowed over the last 5 sessions.   |
|                                                   |
| NVDA currently contributes disproportionately     |
| to sector leadership.                             |
|                                                   |
| Generated: 2026-05-16 08:42 ET                   |
| Source Basis: market snapshot + watchlists       |
+--------------------------------------------------+
```

---

## Anti-Patterns

Avoid:

* advisory panels visually identical to canonical state
* AI action buttons implying autonomous authority
* advisory content mixed directly into lifecycle queues

---

# 5. Context Panel

## Purpose

The Context Panel provides situational awareness.

It answers:

```text id="sn10r3"
What surrounding environmental state matters?
```

---

## Typical Contents

* market regime
* sector movement
* economic events
* earnings windows
* volatility state
* correlation warnings
* breadth indicators
* watchlist movement

---

## Design Guidance

Context Panels should:

* support rapid scanning
* remain compact
* preserve persistence
* avoid excessive narrative verbosity

---

# 6. Replay Panel

## Purpose

Replay Panels support historical reconstruction.

They answer:

```text id="tpzcw6"
What did I know and when did I know it?
```

---

## Typical Contents

* timeline state
* historical context
* lifecycle transitions
* operator actions
* advisory history
* market snapshots
* event chain reconstruction

---

## Design Guidance

Replay Panels should prioritize:

* chronology
* causal clarity
* historical reconstruction
* replay continuity

---

# 7. Review Panel

## Purpose

Review Panels support behavioral and outcome evaluation.

They answer:

```text id="q9m3i8"
What should I learn from this?
```

---

## Typical Contents

* outcome vs thesis
* rule adherence
* mistakes
* successes
* behavioral drift
* lessons learned
* review tags
* pattern references

---

## Design Guidance

Review Panels should feel:

* reflective
* analytical
* behavioral
* evaluative

rather than:

* operationally urgent

---

# 8. Queue Panel

## Purpose

Queue Panels organize operational attention.

They answer:

```text id="3vdghl"
What requires triage?
```

---

## Typical Queue Types

* pending review
* pending approval
* armed attention
* unresolved alerts
* replay backlog
* opportunity intake
* thesis refinement

---

## Design Guidance

Queue Panels should:

* prioritize scanability
* expose urgency clearly
* support rapid triage
* avoid hidden state

---

# 9. Alert Panel

## Purpose

Alert Panels represent elevated operational attention.

They answer:

```text id="t9lxxg"
What requires immediate awareness?
```

---

## Typical Contents

Examples:

* trigger hit
* invalidation breached
* unusual volatility
* earnings proximity
* concentration risk
* behavioral warning
* stale plan state

---

## Design Guidance

Alert Panels should:

* be visually distinct
* preserve seriousness without panic
* avoid excessive interruption patterns
* support operator acknowledgment

---

# 10. Timeline Panel

## Purpose

Timeline Panels support temporal navigation.

They answer:

```text id="u68swt"
How did this operational sequence unfold?
```

---

## Typical Contents

* lifecycle events
* operator actions
* advisory generation
* market snapshots
* rule evaluations
* fill events
* review milestones

---

## Design Guidance

Timeline Panels should prioritize:

* chronology
* state reconstruction
* causality visibility
* replay continuity

---

# 11. Rule Evaluation Panel

## Purpose

Rule Evaluation Panels expose deterministic system evaluation.

They answer:

```text id="j8xmh0"
Did this satisfy operational rules?
```

---

## Typical Contents

* evaluated rules
* pass/fail state
* violation explanations
* supporting evidence
* severity
* historical recurrence

---

## Design Guidance

Rule Evaluation Panels should feel:

* deterministic
* auditable
* explicit
* structured

Avoid vague AI-like language.

---

# 12. Workspace Summary Panel

## Purpose

Workspace Summary Panels aggregate operational overview state.

They answer:

```text id="e3l4vt"
What is the current overall operational situation?
```

---

## Typical Contents

Examples:

* total active positions
* unresolved attention items
* market regime summary
* pending reviews
* active alerts
* exposure overview

---

## Design Guidance

Summary Panels should:

* compress useful operational awareness
* avoid vanity metrics
* avoid dashboard KPI overload
* support quick orientation

---

# Panel Composition Rules

Panels should support:

| Capability            | Preferred          |
| --------------------- | ------------------ |
| Expandable detail     | Yes                |
| Nested tabs           | Sometimes          |
| Inline actions        | Yes                |
| Hover dependency      | Minimal            |
| Independent scrolling | Sometimes          |
| Drag/rearrangement    | Future possibility |
| Persistent state      | Preferred          |

---

# Panel Density Guidance

TradeForge prefers:

```text id="sr4y3t"
operational density
```

over:

```text id="b50a1u"
decorative spaciousness
```

Panels should tolerate:

* compact layouts
* grouped status indicators
* denser metadata
* smaller padding
* multi-column organization

when operationally useful.

---

# Panel Width Guidance

Not all panels should be identical width.

| Panel Type     | Preferred Width |
| -------------- | --------------- |
| Decision Panel | Medium          |
| Position Panel | Medium          |
| Advisory Panel | Medium/Wide     |
| Replay Panel   | Wide            |
| Timeline Panel | Wide            |
| Alert Panel    | Narrow/Medium   |
| Context Panel  | Narrow          |
| Review Panel   | Medium/Wide     |

---

# Authority Layering

Panels must visually reinforce authority distinctions.

| Authority Type | Panel Examples                    |
| -------------- | --------------------------------- |
| Canonical      | Position, Review, Rule Evaluation |
| Operational    | Decision, Queue                   |
| Advisory       | Advisory, Context                 |
| Temporal       | Replay, Timeline                  |
| Behavioral     | Review, Alert                     |

These should not visually collapse into generic interchangeable cards.

---

# Persistence Guidance

| Panel Type     | Persistence Preference |
| -------------- | ---------------------- |
| Queue Panel    | Persistent             |
| Context Panel  | Persistent             |
| Advisory Panel | Semi-persistent        |
| Replay Panel   | Contextual             |
| Detail Panels  | Contextual             |

---

# Progressive Disclosure

Panels should expose:

1. immediate operational summary
2. important status
3. contextual detail
4. expandable deep detail

without forcing deep navigation immediately.

---

# Anti-Patterns

Avoid:

### Giant Monolithic Pages

TradeForge favors operational surfaces, not giant scrolling documents.

---

### Decorative Dashboard Cards

Panels should expose operational meaning, not vanity metrics.

---

### Advisory / Canonical Collapse

AI summaries must not visually resemble lifecycle truth.

---

### Excessive Modal Flows

Panels should preserve workspace continuity.

---

### Hidden Critical State

Important operational state should remain visible.

---

### Mobile-First Compression

Desktop workstation density should remain primary.

---

# Future Evolution

Future panel evolution may include:

* detachable panels
* resizable panels
* workspace persistence
* adaptive density modes
* AI contextual overlays
* multi-monitor awareness
* panel pinning
* operator-specific panel presets
* replay-aware restoration

---

# Implementation Implications

Frontend implementation should eventually support:

* reusable panel primitives
* semantic panel variants
* authority-aware styling
* density controls
* compact operational layouts
* grid-based composition
* contextual persistence
* provenance-aware advisory surfaces

---

# Open Questions

* Should panel layouts be operator-customizable?
* Should certain panels support pinning?
* Should replay panels synchronize across timelines?
* Should advisory panels show confidence gradients?
* Should panels support detached monitor mode?
* Should queue panels support keyboard triage workflows?

---

# Design Invariants

The following remain invariant:

1. Panels are operational cognition surfaces.
2. Canonical and advisory state must remain visually distinct.
3. Panels should maximize useful signal density.
4. Panels should support parallel cognition.
5. Workspaces should preserve operational continuity.
6. Advisory panels must remain non-authoritative.
7. Operational awareness is more important than decorative symmetry.
8. TradeForge is a workstation environment, not a dashboard template.

---

# See Also

* [[workspace-operational-layouts]]
* [[workspace-zoning-reference]]
* [[DESIGN_ARCHITECTURE]]
* [[Operating Workspace]]
* [[Replay Workspace]]
* [[Review Workspace]]
* [[Human Decision Sovereignty]]
* [[AI Advisory Boundary]]

