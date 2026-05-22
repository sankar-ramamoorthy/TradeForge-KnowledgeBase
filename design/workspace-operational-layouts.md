---
title: Workspace Operational Layouts
status: draft
created: 2026-05-16
updated: 2026-05-16
type: design
domain: tradeforge
layer: operational-workspace-architecture
tags:
  - tradeforge
  - design
  - workspace
  - operational-cognition
  - frontend
  - ux
  - layout
  - workstation
related:
  - "[[DESIGN_ARCHITECTURE]]"
  - "[[runtime-context-map]]"
  - "[[WORKSPACES]]"
  - "[[INVARIANTS]]"
  - "[[Human Decision Sovereignty]]"
  - "[[AI Advisory Boundary]]"
  - "[[Replayable Cognition]]"
  - "[[Operating Workspace]]"
  - "[[Replay Workspace]]"
  - "[[Review Workspace]]"
runtime_related:
  - "TradeForge/frontend/DESIGN.md"
  - "TradeForge/frontend/src/styles.css"
  - "TradeForge/frontend/src"
authority: design-doctrine
canonical: false
summary: >
  Defines how TradeForge's operational cognition philosophy becomes concrete workspace
  layout structure, including zoning, density, panel composition, width utilization,
  and canonical/advisory separation.
---

# Workspace Operational Layouts

## Purpose

This document defines the operational layout philosophy for TradeForge workspaces.

TradeForge is not a conventional trading dashboard, form application, or generic admin UI.

TradeForge is an:

```text
operational cognition environment
````

Its workspaces must support:

* structured discretionary decision-making
* replayable cognition
* situational awareness
* lifecycle visibility
* advisory context
* behavioral reinforcement
* disciplined trading execution

The purpose of this document is to define how those ideas become visible workspace structure.

---

## Design Layer

This document occupies the middle layer between design doctrine and frontend implementation.

| Layer                               | File                                                                | Responsibility                                                           |
| ----------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Cognitive doctrine                  | `knowledge-base/TradeForge/design/DESIGN_ARCHITECTURE.md`           | Defines what TradeForge fundamentally means                              |
| Operational workspace architecture  | `knowledge-base/TradeForge/design/workspace-operational-layouts.md` | Defines how operational cognition becomes workspace composition          |
| Frontend implementation translation | `TradeForge/frontend/DESIGN.md`                                     | Defines tokens, spacing, CSS primitives, and visual implementation rules |

This separation is intentional.

TradeForge treats:

```text
meaning
interaction architecture
visual implementation
```

as distinct architectural concerns.

The frontend must translate design doctrine into visible operational surfaces, but it does not own doctrine.

---

## Core Principle

TradeForge layouts optimize for:

```text
parallel cognition
```

rather than:

```text
linear workflow completion
```

The operator should be able to:

* perceive multiple active states at once
* maintain context while acting
* compare active decisions side by side
* notice changes in market or lifecycle state
* preserve cognitive continuity across workflows
* distinguish canonical truth from advisory interpretation

The UI should behave like:

```text
an operational surface
```

not:

```text
a centered CRUD application
```

---

## Primary Design Problem

A narrow centered layout may look clean, but it does not match TradeForge's operational intent.

A centered single-column layout tends to optimize for:

* reading
* forms
* sequential completion
* low-density interaction
* mobile responsiveness

TradeForge needs to optimize for:

* scanning
* monitoring
* comparing
* triaging
* deciding
* reviewing
* replaying

Therefore, desktop workspace layouts should avoid unnecessary dead margins and make deliberate use of available width.

---

## Workspace-Centric Design

TradeForge is workspace-centric, not page-centric.

A page usually answers:

```text
What screen am I on?
```

A workspace answers:

```text
What cognitive mode am I operating in?
```

Examples:

| Workspace                     | Cognitive Mode                    |
| ----------------------------- | --------------------------------- |
| [[Operating Workspace]]       | Active operational awareness      |
| [[Opportunity Workspace]]     | Idea intake and triage            |
| [[Plan Review Workspace]]     | Intent validation                 |
| [[Active Position Workspace]] | Exposure and execution monitoring |
| [[Replay Workspace]]          | Temporal reconstruction           |
| [[Review Workspace]]          | Behavioral and trade review       |

Different workspaces may share components, but they should not be forced into one identical layout shell.

---

## Layout Invariants

The following invariants apply to TradeForge operational layouts:

1. Workspaces are operational surfaces, not documents.
2. Desktop operational awareness is primary.
3. Layouts should support parallel cognition.
4. Useful horizontal width should not be wasted.
5. Canonical state must remain visually distinct from advisory state.
6. Context should remain visible during workflow movement.
7. Panels should preserve decision continuity.
8. Operational density is preferred over decorative whitespace.
9. Human decision sovereignty must be reinforced by the interface.
10. Frontend tokens implement the visual system; they do not define workspace meaning.

---

## Primary Workspace Zones

TradeForge desktop workspaces should generally be composed from four conceptual zones.

```text
+------------+------------------------------+--------------------------+
| Navigation | Primary Operational Surface  | Contextual Awareness     |
| Zone       |                              | Zone                     |
|            |                              |                          |
|            | Secondary Detail Surface     | Advisory / Market / Risk |
+------------+------------------------------+--------------------------+
```

These zones are conceptual. A specific workspace may collapse, expand, or rearrange them.

---

## 1. Navigation Zone

### Purpose

The navigation zone supports workspace switching and operational orientation.

It answers:

```text
Where am I in the system?
```

### Contains

* workspace navigation
* session/operator identity
* system health
* quick status indicators
* high-level unresolved attention count
* global create/action affordances where appropriate

### Design Guidance

The navigation zone should be:

* persistent
* narrow
* visually stable
* low dominance
* available without becoming the main workspace

It should not consume excessive horizontal space.

---

## 2. Primary Operational Surface

### Purpose

The primary operational surface is the main cognitive work area.

It answers:

```text
What requires attention now?
```

### Contains

Depending on workspace:

* active decisions
* armed trade plans
* active positions
* pending reviews
* rule evaluations
* lifecycle status
* decision queues
* execution readiness
* plan state
* operator actions

### Design Guidance

The primary operational surface should:

* use the largest share of screen real estate
* support multiple panels
* support dense but readable cards
* make active state visible without excessive clicking
* avoid unnecessary centered-column constraints
* preserve workflow continuity

---

## 3. Contextual Awareness Zone

### Purpose

The contextual awareness zone provides non-canonical situational awareness.

It answers:

```text
What surrounding context should influence my attention?
```

### Contains

Examples:

* market regime
* sector context
* watchlist state
* upcoming earnings
* macro events
* economic calendar
* news summaries
* volatility context
* advisory overlays
* AI-generated context
* behavioral warnings
* correlation or concentration warnings

### Authority Boundary

This zone is advisory.

It must not appear to be canonical lifecycle truth.

It may inform decisions, but it must not approve, execute, mutate, or finalize lifecycle transitions.

### Design Guidance

The contextual awareness zone should:

* usually appear as a persistent right rail on desktop
* be visually distinct from canonical workflow surfaces
* support refresh/recompute behavior
* preserve provenance when context is advisory or AI-generated
* avoid implying execution authority

---

## 4. Secondary Detail Surface

### Purpose

The secondary detail surface supports focused inspection.

It answers:

```text
What details do I need before deciding?
```

### Contains

Examples:

* full trade thesis
* plan details
* lifecycle event history
* replay timeline
* rule evaluation details
* review notes
* fill history
* advisory provenance
* linked artifacts

### Design Guidance

The secondary detail surface may be:

* expandable
* collapsible
* drawer-based
* lower panel
* side panel
* tabbed inside a selected object

It should support depth without destroying workspace awareness.

---

## Width Utilization

Desktop layouts should make intentional use of available horizontal space.

Avoid defaulting to:

```text
max-width centered document shell
```

for operational workspaces.

Prefer:

```text
full-width operational shell
```

with constrained internal panels.

### Bad Pattern

```text
| huge margin | narrow centered card | huge margin |
```

This creates visual cleanliness but wastes operational awareness.

### Preferred Pattern

```text
| nav | primary work panels | context rail |
```

This allows TradeForge to behave as a workstation.

---

## Panel Philosophy

TradeForge should prefer panels over long stacked sections when multiple operational objects are active.

### Weak Pattern

```text
SMH
NFLX
NVDA
```

### Stronger Pattern

```text
+----------+ +----------+ +----------+
| SMH      | | NFLX     | | NVDA     |
| Armed    | | Idea     | | Armed    |
| Trigger  | | Thesis   | | Trigger  |
| Risk     | | Context  | | Risk     |
+----------+ +----------+ +----------+
```

Panels support:

* comparison
* triage
* monitoring
* parallel cognition
* quick operator attention shifts

---

## Information Density

TradeForge should be dense but readable.

Avoid:

* decorative whitespace
* oversized cards
* oversized typography
* excessive vertical stacking
* hidden operational status
* dashboard vanity metrics

Prefer:

* compact cards
* grouped operational state
* clear section hierarchy
* inline status markers
* glanceable summaries
* progressive detail expansion

The interface should support:

```text
fast operational scanning
```

rather than:

```text
slow sequential reading
```

---

## Canonical / Operational / Advisory / Behavioral Layers

The UI must visually distinguish among different kinds of state.

| Layer       | Meaning                       | Examples                                  | UI Implication                         |
| ----------- | ----------------------------- | ----------------------------------------- | -------------------------------------- |
| Canonical   | Durable system truth          | lifecycle events, approved plans, reviews | strongest authority treatment          |
| Operational | Current workflow state        | armed, pending, attention queues          | active workspace treatment             |
| Advisory    | AI or market interpretation   | briefing, summaries, suggested risks      | visually distinct and provenance-aware |
| Behavioral  | Discipline and review signals | repeated violations, thesis drift         | warning/reflective treatment           |
| Contextual  | External environment          | market regime, news, earnings             | non-authoritative context treatment    |

These layers must not collapse visually into a generic card style.

---

## Human Decision Sovereignty

TradeForge must reinforce [[Human Decision Sovereignty]].

The UI must make clear that:

* the operator decides
* AI advises
* market data informs
* lifecycle services mutate canonical state
* execution facts are recorded, not invented
* advisory context is not authority

The interface should avoid language, button placement, or visual treatment that implies autonomous execution authority.

---

## AI Advisory Boundary

AI-generated or AI-assisted content must be clearly separated from canonical workflow state.

AI advisory surfaces should show, where appropriate:

* provenance
* timestamp
* input basis
* confidence or uncertainty
* non-authoritative status
* operator review affordance

AI outputs should not be visually styled as lifecycle truth.

AI must not be able to:

* approve plans
* arm trades
* open positions
* close positions
* create canonical reviews
* mutate lifecycle state directly

---

## Operating Workspace Layout

### Cognitive Purpose

The Operating Workspace supports active daily trading awareness.

It answers:

```text
What requires attention now?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Attention Queue               | Market Context            |
|            | Active Decisions              | Watchlist / Regime        |
|            | Armed Plans                   | Advisory Briefing         |
|            | Active Positions              | Events / Earnings         |
|            | Pending Reviews               | Behavioral Warnings       |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* attention queue
* active decisions
* armed plans
* active positions
* pending review queue
* contextual briefing
* market state
* operational status

### Layout Notes

The Operating Workspace should be the most workstation-like workspace.

It should avoid a narrow centered single-column layout.

---

## Opportunity Workspace Layout

### Cognitive Purpose

The Opportunity Workspace supports intake and triage of trade ideas.

It answers:

```text
What opportunities are worth developing?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Idea Inbox / Watchlist        | Market / Sector Context   |
|            | Candidate Opportunity Cards   | Recent News               |
|            | Triage State                  | AI Idea Feeder Notes      |
|            | Promotion Actions             | Rejection Reasons         |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* idea inbox
* watchlist candidates
* opportunity cards
* triage labels
* source/provenance
* promotion to thesis
* rejection/defer notes

---

## Plan Review Workspace Layout

### Cognitive Purpose

The Plan Review Workspace supports validation of intent before a plan becomes active.

It answers:

```text
Is this trade plan coherent enough to arm?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Plan Under Review             | Rule Evaluation           |
|            | Thesis / Setup / Trigger      | Risk Context              |
|            | Entry / Exit / Invalidations  | Advisory Critique         |
|            | Operator Approval Actions     | Similar Past Trades       |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* thesis summary
* plan details
* trigger conditions
* invalidation
* sizing assumptions
* rule evaluations
* approval/arm decision
* advisory critique

---

## Active Position Workspace Layout

### Cognitive Purpose

The Active Position Workspace supports monitoring open exposure.

It answers:

```text
What is happening to my active risk?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Open Positions                | Market / News Context     |
|            | Position State                | Risk / Exposure Summary   |
|            | Fills / Quantity / Basis      | Thesis Drift Signals      |
|            | Stop / Target / Invalidation  | Advisory Warnings         |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* open positions
* fill history
* position quantity
* average entry
* current plan relationship
* thesis drift indicators
* exit readiness
* exposure/risk summary

---

## Replay Workspace Layout

### Cognitive Purpose

The Replay Workspace supports temporal reconstruction.

It answers:

```text
What happened, in what order, and what did I know at the time?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Event Timeline                | Context Snapshot          |
|            | Lifecycle Events              | Market State At Time      |
|            | Decision Points               | Advisory Artifacts        |
|            | State Reconstruction          | Notes / Evidence          |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* timeline
* lifecycle events
* state reconstruction
* market snapshots
* advisory history
* operator notes
* replay controls

---

## Review Workspace Layout

### Cognitive Purpose

The Review Workspace supports learning and behavioral improvement.

It answers:

```text
What should I learn from this trade or pattern?
```

### Preferred Desktop Layout

```text
+------------+-------------------------------+---------------------------+
| Nav        | Trade Review                  | Pattern Analytics         |
|            | Journal / Reflection          | Rule Violations           |
|            | Outcome vs Intent             | Behavioral Signals        |
|            | Lessons / Tags                | Similar Historical Trades |
+------------+-------------------------------+---------------------------+
```

### Key Surfaces

* trade review
* outcome vs thesis
* rule adherence
* mistake classification
* behavior tags
* lessons learned
* pattern analytics

---

## Responsive Strategy

TradeForge is desktop-first.

Mobile and narrow layouts may collapse zones into sequential sections, but this is a degradation strategy.

Desktop remains the canonical operational experience.

### Desktop

* persistent nav
* primary surface
* contextual awareness rail
* multi-panel layout
* high information density

### Tablet / Narrow

* collapsible context rail
* fewer simultaneous panels
* detail drawers

### Mobile

* sequential workflows
* simplified cards
* minimal monitoring
* review/entry convenience only

Mobile should not define the primary workspace model.

---

## Relationship To `frontend/DESIGN.md`

`TradeForge/frontend/DESIGN.md` defines frontend implementation primitives such as:

* colors
* spacing
* typography
* radius
* layout constraints
* CSS custom property names
* reusable visual rules

This document defines the operational layout model those primitives must serve.

Frontend implementation should avoid treating current token values as proof that the layout model is correct.

A visually consistent layout can still be operationally wrong if it:

* wastes workspace width
* hides active state
* collapses advisory and canonical state
* forces sequential cognition
* removes persistent context
* treats workspaces like pages

---

## Relationship To Runtime Architecture

The UI is not merely presentation.

It is the visible projection of runtime meaning.

Runtime concepts that must remain visible or recoverable in the UI include:

* lifecycle state
* event history
* rule evaluation
* plan approval status
* armed state
* position state
* fill-derived position truth
* review immutability
* advisory provenance
* operator decision boundaries

The layout must preserve these distinctions.

---

## Anti-Patterns

Avoid the following:

### Centered App Shell For Operational Workspaces

A narrow centered workspace with large dead margins weakens situational awareness.

### Generic Dashboard Metric Grid

TradeForge is not primarily a KPI dashboard.

Metrics are useful only when tied to operational cognition.

### Advisory / Canonical Visual Collapse

AI summaries, market notes, and lifecycle truth must not look equally authoritative.

### Modal Overuse

Modals interrupt cognition and hide context.

Prefer inline panels, drawers, or dedicated detail surfaces.

### Hidden State Behind Clicks

Important operational state should be visible without excessive drilling.

### Mobile-First Constraint Leakage

Do not let mobile layout constraints dominate the desktop workstation experience.

---

## Implementation Implications

Future frontend work should consider:

* full-width operational shells
* CSS grid-based workspace zones
* persistent right context rail
* compact operational cards
* multi-column active decision views
* selectable/resizable panels
* saved workspace configurations
* advisory provenance components
* canonical/advisory visual separation
* less reliance on global centered max-width shells

---

## Open Questions

* Should workspaces eventually support user-resizable panels?
* Should right-rail context be globally persistent or workspace-specific?
* Should workspace layouts be saved per operator?
* Should TradeForge support multi-monitor workspace presets?
* Should advisory panels have a standardized provenance footer?
* Should attention queues become the primary organizing pattern for the Operating Workspace?
* Should mobile be read/review only for early versions?

---

## See Also

* [[DESIGN_ARCHITECTURE]]
* [[runtime-context-map]]
* [[WORKSPACES]]
* [[INVARIANTS]]
* [[Human Decision Sovereignty]]
* [[AI Advisory Boundary]]
* [[Replayable Cognition]]
* [[Operating Workspace]]
* [[Replay Workspace]]
* [[Review Workspace]]

