---
title: Workspace Zoning Reference
status: draft
created: 2026-05-16
updated: 2026-05-16
type: design
domain: tradeforge
layer: workspace-zoning
tags:
  - tradeforge
  - workspace
  - zoning
  - frontend
  - operational-cognition
  - ui
  - layout
  - workstation
related:
  - "[[workspace-operational-layouts]]"
  - "[[DESIGN_ARCHITECTURE]]"
  - "[[runtime-context-map]]"
  - "[[Operating Workspace]]"
  - "[[Replay Workspace]]"
  - "[[Review Workspace]]"
  - "[[Human Decision Sovereignty]]"
  - "[[AI Advisory Boundary]]"
runtime_related:
  - "TradeForge/frontend/DESIGN.md"
  - "TradeForge/frontend/src/styles.css"
  - "TradeForge/frontend/src"
authority: design-doctrine
canonical: false
summary: >
  Defines the operational zoning model for TradeForge workspaces, including
  structural workspace regions, behavioral expectations, persistence rules,
  contextual awareness boundaries, and desktop workstation composition.
---

# Workspace Zoning Reference

## Purpose

This document defines the zoning model used by TradeForge operational workspaces.

A workspace is not merely a page layout.

It is a:

```text
structured cognitive environment
````

designed to support:

* discretionary decision-making
* operational continuity
* situational awareness
* replayable cognition
* disciplined execution
* behavioral reinforcement
* advisory augmentation

Zoning defines how workspace regions are organized to support those goals.

---

# Core Principle

TradeForge zones exist to support:

```text
parallel operational cognition
```

not:

```text
linear document reading
```

A workspace should allow the operator to:

* act
* monitor
* compare
* inspect
* review
* contextualize

simultaneously.

---

# Relationship To Other Design Documents

| Document                           | Responsibility                                           |
| ---------------------------------- | -------------------------------------------------------- |
| `DESIGN_ARCHITECTURE.md`           | Defines cognitive and operational philosophy             |
| `workspace-operational-layouts.md` | Defines overall workspace composition philosophy         |
| `workspace-zoning-reference.md`    | Defines structural workspace regions and zoning behavior |
| `frontend/DESIGN.md`               | Defines implementation primitives and tokens             |

This document focuses specifically on:

```text
workspace spatial organization
```

---

# Zoning Philosophy

TradeForge workspaces should behave like:

* operational workstations
* mission-control environments
* cognitive surfaces
* multi-context operational consoles

They should not behave like:

* centered forms
* blogs
* generic dashboards
* mobile-first admin panels

Zones exist to preserve:

* awareness
* continuity
* authority separation
* operational density
* contextual persistence

---

# Canonical Workspace Model

Most desktop workspaces conceptually follow this structure:

```text
+------------+--------------------------------+--------------------------+
|            |                                |                          |
| Navigation | Primary Operational Surface    | Contextual Awareness     |
| Zone       |                                | Zone                     |
|            |                                |                          |
+------------+--------------------------------+--------------------------+
|            |                                |                          |
|            | Secondary Detail Surface       | Optional Detail Context  |
|            |                                |                          |
+------------+--------------------------------+--------------------------+
```

Specific workspaces may collapse, reorder, or emphasize different zones.

---

# Zone Types

TradeForge currently defines four primary workspace zone categories.

| Zone                        | Purpose                                         |
| --------------------------- | ----------------------------------------------- |
| Navigation Zone             | Workspace orientation and movement              |
| Primary Operational Surface | Active operational cognition                    |
| Contextual Awareness Zone   | Persistent advisory and environmental awareness |
| Secondary Detail Surface    | Focused inspection and deep detail              |

---

# 1. Navigation Zone

## Purpose

The Navigation Zone provides:

* orientation
* workspace switching
* operational anchoring
* global state awareness

It answers:

```text
Where am I and what operational mode am I in?
```

---

## Typical Contents

* workspace selector
* navigation rail
* operator identity
* system health
* session information
* unresolved attention count
* quick-create affordances
* workspace switching controls

---

## Structural Characteristics

| Property         | Guidance                      |
| ---------------- | ----------------------------- |
| Persistence      | Persistent                    |
| Width            | Narrow                        |
| Visual Weight    | Low                           |
| Position         | Usually left rail             |
| Scroll Behavior  | Independent when needed       |
| Collapse Support | Yes                           |
| Mobile Behavior  | Drawer or collapsible overlay |

---

## Design Goals

The Navigation Zone should:

* remain stable during workflow movement
* avoid excessive visual dominance
* support fast workspace switching
* preserve orientation
* avoid consuming primary cognitive space

---

## Anti-Patterns

Avoid:

* oversized sidebars
* decorative navigation shells
* workspace titles consuming excessive vertical space
* large dead regions
* duplicated navigation structures

---

# 2. Primary Operational Surface

## Purpose

The Primary Operational Surface is the core operational work area.

It answers:

```text
What requires my active attention?
```

This is the center of:

* operational cognition
* decision-making
* monitoring
* execution readiness
* workflow progression

---

## Typical Contents

Depending on workspace:

* active decisions
* armed plans
* positions
* lifecycle queues
* operational summaries
* trade panels
* approval actions
* review actions
* timeline surfaces
* replay controls

---

## Structural Characteristics

| Property            | Guidance                 |
| ------------------- | ------------------------ |
| Width               | Largest workspace region |
| Density             | High                     |
| Flexibility         | Adaptive                 |
| Layout Model        | Grid or panel-based      |
| Scroll Behavior     | Workspace-dependent      |
| Multi-Panel Support | Required                 |
| Resizable Potential | Likely future support    |

---

## Design Goals

The Primary Operational Surface should:

* maximize useful signal density
* preserve visibility of active state
* support comparison across objects
* reduce unnecessary navigation
* avoid narrow centered layouts
* support multi-panel cognition

---

## Preferred Layout Patterns

### Good

```text
+-----------+ +-----------+ +-----------+
| SMH       | | NVDA      | | NFLX      |
| Armed     | | Position  | | Idea      |
| Trigger   | | P/L       | | Thesis    |
+-----------+ +-----------+ +-----------+
```

### Weak

```text
SMH
NVDA
NFLX
```

---

## Anti-Patterns

Avoid:

* excessive whitespace
* document-style centered containers
* excessive modal workflows
* stacked operational cards with no comparison support
* deep drilldown dependency for basic state awareness

---

# 3. Contextual Awareness Zone

## Purpose

The Contextual Awareness Zone preserves environmental and advisory awareness.

It answers:

```text
What surrounding context should influence my judgment?
```

This zone supports awareness without becoming authority.

---

## Typical Contents

Examples:

* market regime
* watchlists
* macro events
* earnings calendar
* sector movement
* volatility state
* AI advisory overlays
* risk concentration
* behavioral reminders
* correlation warnings
* news summaries

---

## Authority Boundary

This zone is:

```text
advisory
```

not:

```text
canonical
```

It must never imply lifecycle authority.

---

## Structural Characteristics

| Property         | Guidance                |
| ---------------- | ----------------------- |
| Position         | Usually right rail      |
| Persistence      | Prefer persistent       |
| Width            | Medium                  |
| Density          | Moderate                |
| Refresh Behavior | Dynamic                 |
| Scroll Behavior  | Independent when useful |
| Mobile Behavior  | Collapsible or hidden   |

---

## Design Goals

The Contextual Awareness Zone should:

* preserve situational continuity
* remain visually distinct from canonical state
* support rapid scanning
* avoid overpowering operational workflow
* support provenance visibility

---

## Visual Distinction Requirements

Advisory surfaces should visually differ from:

* lifecycle truth
* execution state
* immutable reviews
* canonical operational status

The operator must always understand:

```text
this is context, not authority
```

---

## Anti-Patterns

Avoid:

* AI summaries visually identical to lifecycle state
* advisory cards mixed into canonical queues
* aggressive AI action affordances
* context dominating operational surfaces

---

# 4. Secondary Detail Surface

## Purpose

The Secondary Detail Surface supports focused inspection.

It answers:

```text
What deeper detail do I need right now?
```

---

## Typical Contents

Examples:

* full thesis
* lifecycle history
* replay event chains
* fill details
* review notes
* rule evaluation details
* advisory provenance
* linked artifacts
* execution notes

---

## Structural Characteristics

| Property          | Guidance                     |
| ----------------- | ---------------------------- |
| Persistence       | Optional                     |
| Expansion         | Supported                    |
| Collapse          | Supported                    |
| Position          | Flexible                     |
| Interaction Style | Focus-oriented               |
| Layout Style      | Drawer/panel/tab/detail view |

---

## Design Goals

The Secondary Detail Surface should:

* provide depth without destroying awareness
* preserve operational continuity
* support inspection without modal overload
* allow return to primary workflow easily

---

## Anti-Patterns

Avoid:

* full-screen modal takeover
* detail flows that destroy workspace continuity
* hidden return paths
* deeply nested navigation chains

---

# Workspace Density Model

TradeForge workspaces should prefer:

```text
dense but readable operational layouts
```

rather than:

```text
decorative sparse layouts
```

---

## Density Goals

| Goal          | Meaning                                    |
| ------------- | ------------------------------------------ |
| Glanceability | Important state visible quickly            |
| Scanability   | Operator can visually triage rapidly       |
| Continuity    | State remains visible during action        |
| Compression   | Useful information per screen area         |
| Separation    | Different authority layers remain distinct |

---

## Acceptable Density

TradeForge should tolerate:

* compact cards
* tighter spacing
* multi-column operational layouts
* persistent context rails
* grouped state indicators

when they improve operational awareness.

---

# Width Allocation Guidance

Desktop operational workspaces should intentionally use available width.

---

## Preferred Desktop Ratio

Approximate conceptual ratio:

```text
[ Navigation ] [ Primary Operational Surface ] [ Context Rail ]
      15%                    60%                      25%
```

Not strict CSS values — conceptual guidance only.

---

## Avoid

```text
| large empty margin | narrow card | large empty margin |
```

This weakens operational cognition.

---

# Persistence Rules

Some workspace zones should remain persistent whenever possible.

| Zone                        | Persistence Preference |
| --------------------------- | ---------------------- |
| Navigation Zone             | Persistent             |
| Primary Operational Surface | Persistent             |
| Contextual Awareness Zone   | Prefer persistent      |
| Secondary Detail Surface    | Context-dependent      |

---

# Scroll Behavior

TradeForge should support independent scrolling where useful.

Examples:

| Zone                        | Independent Scroll? |
| --------------------------- | ------------------- |
| Navigation Zone             | Sometimes           |
| Primary Operational Surface | Often               |
| Contextual Awareness Zone   | Often               |
| Secondary Detail Surface    | Usually             |

Independent scrolling supports operational continuity.

---

# Collapse Rules

Zones may support collapse behavior.

| Zone                        | Collapse Support |
| --------------------------- | ---------------- |
| Navigation Zone             | Yes              |
| Contextual Awareness Zone   | Yes              |
| Secondary Detail Surface    | Yes              |
| Primary Operational Surface | No               |

The Primary Operational Surface should remain dominant.

---

# Responsive Degradation Strategy

TradeForge is desktop-first.

Mobile layouts are degradation paths, not primary design targets.

---

## Desktop

Supports:

* persistent zones
* multi-panel layouts
* high-density operational cognition
* workstation behavior

---

## Tablet / Narrow Width

May collapse:

* contextual awareness rail
* secondary detail surface

while preserving operational continuity.

---

## Mobile

Mobile layouts may:

* stack zones sequentially
* reduce simultaneous visibility
* prioritize quick review or updates
* defer heavy operational workflows

Mobile should not dictate desktop architecture.

---

# Zoning And Authority Separation

Workspace zones reinforce architectural authority boundaries.

| Zone                        | Typical Authority Character |
| --------------------------- | --------------------------- |
| Navigation Zone             | Structural                  |
| Primary Operational Surface | Canonical + operational     |
| Contextual Awareness Zone   | Advisory/contextual         |
| Secondary Detail Surface    | Mixed depending on artifact |

The UI must preserve these distinctions visually and behaviorally.

---

# Future Evolution

Future zoning evolution may include:

* resizable panels
* detachable workspace regions
* multi-monitor awareness
* saved workspace presets
* operator-specific layouts
* adaptive density modes
* focus mode
* replay-aware restoration
* AI contextual overlays
* heatmaps and alert gradients

---

# Implementation Implications

Frontend implementation should eventually support:

* CSS grid workspace shells
* persistent right context rail
* flexible panel sizing
* panel composition primitives
* independent scrolling regions
* responsive zone collapse
* contextual awareness persistence
* operational density controls

---

# Open Questions

* Should users be able to resize operational zones?
* Should the right context rail remain globally persistent?
* Should zoning differ significantly by persona?
* Should zones support drag-and-drop rearrangement?
* Should replay workspaces support timeline docking?
* Should advisory context be pin-able?
* Should workspace presets be serializable?

---

# Design Invariants

The following remain invariant:

1. Workspaces are operational cognition surfaces.
2. Parallel cognition is preferred over sequential interaction.
3. Canonical and advisory state must remain distinguishable.
4. Desktop operational awareness is primary.
5. Context should persist whenever possible.
6. Width is a strategic resource, not decorative whitespace.
7. Zones exist to support cognition, not visual symmetry.
8. TradeForge is a workstation environment, not a document viewer.

---

# See Also

* [[workspace-operational-layouts]]
* [[DESIGN_ARCHITECTURE]]
* [[runtime-context-map]]
* [[Operating Workspace]]
* [[Replay Workspace]]
* [[Review Workspace]]
* [[Human Decision Sovereignty]]
* [[AI Advisory Boundary]]

