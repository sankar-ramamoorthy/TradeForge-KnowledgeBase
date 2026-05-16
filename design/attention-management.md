---
title: Attention Management
status: draft
created: 2026-05-16
updated: 2026-05-16
type: design
domain: tradeforge
layer: operational-attention-architecture
tags:
  - tradeforge
  - design
  - attention
  - cognition
  - operational-awareness
  - alerts
  - behavioral
  - workflow
  - ui
related:
  - "[[DESIGN_ARCHITECTURE]]"
  - "[[workspace-operational-layouts]]"
  - "[[workspace-zoning-reference]]"
  - "[[operational-panel-patterns]]"
  - "[[Human Decision Sovereignty]]"
  - "[[AI Advisory Boundary]]"
  - "[[Replayable Cognition]]"
  - "[[Operating Workspace]]"
runtime_related:
  - "TradeForge/frontend/DESIGN.md"
  - "TradeForge/frontend/src"
authority: design-doctrine
canonical: false
summary: >
  Defines how TradeForge manages operational attention, alerts, interruptions,
  cognitive load, situational awareness, and behavioral signaling while preserving
  human decision sovereignty and workstation-oriented operational cognition.
---

# Attention Management

## Purpose

This document defines the attention management philosophy used throughout TradeForge.

TradeForge is not designed to maximize:

```text
engagement
````

It is designed to maximize:

```text id="3yds83"
disciplined operational cognition
```

Attention management exists to help the operator:

* maintain situational awareness
* detect important changes
* prioritize meaningful signals
* reduce cognitive overload
* preserve workflow continuity
* reinforce disciplined execution
* avoid impulsive behavior

TradeForge should support:

```text id="opcf8m"
intentional attention
```

not:

```text id="r9fuvk"
attention extraction
```

---

# Relationship To Other Design Documents

| Document                           | Responsibility                                       |
| ---------------------------------- | ---------------------------------------------------- |
| `DESIGN_ARCHITECTURE.md`           | Cognitive philosophy                                 |
| `workspace-operational-layouts.md` | Workspace composition                                |
| `workspace-zoning-reference.md`    | Spatial workspace organization                       |
| `operational-panel-patterns.md`    | Reusable operational surfaces                        |
| `attention-management.md`          | Attention prioritization and interruption philosophy |

This document focuses specifically on:

```text id="88zx1m"
operational attention flow
```

---

# Core Principle

TradeForge attention systems optimize for:

```text id="gwhmp5"
situational awareness
```

rather than:

```text id="h8ik8w"
constant stimulation
```

The interface should help the operator answer:

```text id="mkm89j"
What actually matters right now?
```

without overwhelming them with noise.

---

# Attention Philosophy

TradeForge assumes:

* human attention is finite
* excessive interruption degrades judgment
* constant alerting weakens discipline
* context switching has cognitive cost
* not all signals deserve equal urgency
* operational clarity is more important than visual excitement

The system should prefer:

```text id="xq1mpq"
calm operational awareness
```

over:

```text id="7yb8e8"
hyperactive notification behavior
```

---

# Attention Hierarchy

TradeForge attention systems should classify operational signals into distinct levels.

| Level         | Meaning                         |
| ------------- | ------------------------------- |
| Informational | Awareness only                  |
| Contextual    | Relevant but not urgent         |
| Operational   | Requires workflow attention     |
| Elevated      | Requires timely operator review |
| Critical      | Immediate awareness required    |

These levels should influence:

* visual prominence
* persistence
* interruption behavior
* notification strategy
* workspace placement

---

# 1. Informational Attention

## Purpose

Informational signals support awareness without demanding action.

---

## Examples

* market breadth update
* sector movement
* watchlist movement
* historical note
* informational advisory summary
* low-priority reminders

---

## Behavioral Rules

Informational signals should:

* remain low visual intensity
* avoid interruption
* remain skimmable
* support optional review
* avoid workflow disruption

---

## Visual Guidance

Informational signals should:

* avoid aggressive colors
* avoid motion
* avoid modal interruption
* avoid repeated resurfacing

---

# 2. Contextual Attention

## Purpose

Contextual signals provide relevant environmental awareness.

They answer:

```text id="qrz7nr"
What surrounding context may influence judgment?
```

---

## Examples

* earnings approaching
* macro event upcoming
* volatility expansion
* regime transition
* unusual sector divergence
* concentration risk emerging

---

## Behavioral Rules

Contextual signals should:

* remain visible
* preserve continuity
* avoid panic escalation
* remain advisory
* support quick scanning

---

## Placement Guidance

Contextual signals should typically appear in:

* contextual awareness rails
* market context panels
* workspace briefing panels

rather than:

* modal alerts
* blocking interruptions

---

# 3. Operational Attention

## Purpose

Operational attention signals indicate meaningful workflow state.

They answer:

```text id="6rqqny"
What operational object requires review or action?
```

---

## Examples

* armed plan awaiting trigger
* pending review
* incomplete thesis
* unresolved workflow state
* pending rule evaluation
* replay awaiting completion

---

## Behavioral Rules

Operational attention should:

* remain persistently visible
* support triage
* preserve workflow continuity
* avoid excessive urgency styling
* remain organized through queues

---

## Preferred Patterns

Operational attention should prefer:

```text id="vzw2ma"
attention queues
```

rather than:

```text id="8pjlwm"
interruptive popups
```

---

# 4. Elevated Attention

## Purpose

Elevated attention signals indicate situations that require timely awareness.

They answer:

```text id="8rfxvf"
What needs prompt operator evaluation?
```

---

## Examples

* trigger condition met
* invalidation nearly breached
* concentrated exposure detected
* stale armed plan
* unusual volatility event
* behavioral drift warning
* correlated position expansion

---

## Behavioral Rules

Elevated attention should:

* stand out visually
* remain persistent until acknowledged
* avoid panic escalation
* avoid repetitive interruption loops
* preserve operator control

---

## Preferred Interaction Style

Prefer:

* persistent workspace visibility
* acknowledgment controls
* escalation queues
* highlighted panels

Avoid:

* flashing UI
* aggressive motion
* repeated sound alerts
* modal interruption loops

---

# 5. Critical Attention

## Purpose

Critical attention signals indicate potentially dangerous operational situations.

They answer:

```text id="3k39mu"
What requires immediate awareness?
```

---

## Examples

* stop breach
* catastrophic exposure condition
* severe data inconsistency
* replay integrity failure
* execution mismatch
* broken lifecycle state
* severe risk concentration

---

## Behavioral Rules

Critical attention may:

* interrupt workflows
* override lower-priority surfaces
* require acknowledgment
* temporarily dominate workspace attention

However, TradeForge should still avoid:

* panic-oriented design
* emotional escalation
* manipulative urgency patterns

---

# Attention Queues

TradeForge should organize most operational attention through queues.

Queues support:

* triage
* prioritization
* continuity
* operational calmness
* reduced interruption frequency

---

# Typical Queue Types

| Queue                 | Purpose                          |
| --------------------- | -------------------------------- |
| Pending Review Queue  | Awaiting trade review            |
| Armed Attention Queue | Armed plans requiring monitoring |
| Opportunity Queue     | Candidate ideas                  |
| Alert Queue           | Elevated operational events      |
| Replay Queue          | Pending replay/reconstruction    |
| Behavioral Queue      | Recurring discipline issues      |
| Rule Evaluation Queue | Pending validation state         |

---

# Queue Philosophy

Queues are preferred because they:

* reduce interruption frequency
* preserve operator agency
* support batch cognition
* maintain operational calmness
* prevent alert fragmentation

TradeForge should avoid:

```text id="fzh7dr"
notification-driven chaos
```

---

# Alert Philosophy

TradeForge alerts should:

* inform
* prioritize
* contextualize
* reinforce discipline

They should not:

* manipulate
* gamify
* stimulate compulsive checking
* encourage impulsive action

---

# Alert Persistence

| Alert Type    | Persistence                   |
| ------------- | ----------------------------- |
| Informational | Temporary                     |
| Contextual    | Semi-persistent               |
| Operational   | Persistent until resolved     |
| Elevated      | Persistent until acknowledged |
| Critical      | Persistent until resolved     |

---

# Alert Escalation

TradeForge should support escalation based on:

* severity
* duration
* recurrence
* operator inactivity
* operational importance

Escalation should be:

```text id="yq5y20"
measured and explainable
```

not:

```text id="fg5j31"
emotionally manipulative
```

---

# Escalation Anti-Patterns

Avoid:

* flashing animations
* constant pulsing
* excessive sound
* repeated modal popups
* red saturation overload
* "urgent trading" gamification
* emotionally loaded language

---

# Behavioral Attention

TradeForge includes behavioral cognition as a first-class concern.

Behavioral attention exists to surface:

* repeated mistakes
* discipline drift
* impulsive patterns
* thesis inconsistency
* rule violations
* recurring operational weaknesses

---

# Behavioral Signals

Examples:

* repeatedly moving stops
* entering before trigger
* chasing extended setups
* ignoring invalidation
* overconcentration
* revenge trading tendencies
* skipping reviews

---

# Behavioral Design Guidance

Behavioral signals should:

* remain calm
* remain evidence-based
* avoid shame-oriented design
* support reflection
* encourage review

TradeForge should reinforce:

```text id="wgxb6r"
disciplined self-awareness
```

not:

```text id="up4s84"
emotional punishment
```

---

# AI Advisory Attention

AI-generated attention surfaces must remain:

```text id="s9vb9h"
advisory
```

not:

```text id="smqexl"
authoritative
```

---

# AI Attention Examples

Examples:

* "This setup resembles prior failed momentum breakouts."
* "Concentration risk has increased."
* "This thesis conflicts with current regime assumptions."

---

# AI Attention Rules

AI-generated attention should:

* show provenance
* show uncertainty where relevant
* avoid imperative language
* avoid autonomous urgency
* avoid execution authority implication

---

# Attention Placement Rules

| Attention Type | Preferred Placement             |
| -------------- | ------------------------------- |
| Informational  | Context rail                    |
| Contextual     | Context panels                  |
| Operational    | Attention queues                |
| Elevated       | Persistent highlighted panels   |
| Critical       | Blocking/high-priority surfaces |

---

# Motion Philosophy

TradeForge should use motion sparingly.

Motion should support:

* orientation
* continuity
* transition clarity

Motion should not be used to:

* manufacture urgency
* increase engagement
* create stimulation loops

---

# Sound Philosophy

Sound should be:

* optional
* sparse
* severity-aware
* operator-controlled

Default environments should remain calm.

---

# Color Philosophy

Color should indicate:

* operational meaning
* severity
* authority type

not emotional manipulation.

Avoid:

* excessive red saturation
* casino-like urgency
* attention farming color schemes

---

# Cognitive Load Management

TradeForge should actively reduce unnecessary cognitive load.

---

## Cognitive Load Risks

Examples:

* excessive alerts
* duplicated information
* conflicting advisory surfaces
* dense unreadable layouts
* unclear authority boundaries
* constant context switching

---

## Mitigation Strategies

TradeForge should prefer:

* grouped signals
* attention queues
* progressive disclosure
* persistent context
* calm escalation
* workspace continuity

---

# Focus Preservation

TradeForge should help the operator maintain focus.

Avoid:

* unnecessary modal interruption
* attention hijacking
* unrelated alert surfacing
* disruptive auto-navigation

Prefer:

* inline escalation
* queue accumulation
* acknowledgment flows
* persistent context surfaces

---

# Replay-Aware Attention

Attention systems should preserve replayability.

The operator should later be able to understand:

* what surfaced
* why it surfaced
* when it surfaced
* what severity existed
* what advisory basis existed

Attention itself becomes part of replayable cognition.

---

# Attention Provenance

Important operational attention should preserve:

* source
* timestamp
* triggering condition
* advisory basis
* escalation reason

This is especially important for:

* AI-generated signals
* behavioral warnings
* risk escalation

---

# Responsive Attention Strategy

Desktop remains the canonical operational environment.

Mobile attention behavior should be reduced and simplified.

---

## Desktop

Supports:

* persistent queues
* persistent context rails
* multi-panel monitoring
* operational density

---

## Mobile

Should prioritize:

* review
* acknowledgment
* minimal interruption
* simplified operational awareness

Avoid:

* aggressive mobile notification patterns
* over-fragmented alerts
* attention overload

---

# Future Evolution

Future attention architecture may include:

* adaptive attention scoring
* replay-aware escalation history
* operator-specific attention tuning
* behavioral fatigue detection
* focus mode
* quiet operational windows
* context-aware escalation thresholds
* AI summarization of queue clusters
* multi-monitor awareness

---

# Anti-Patterns

Avoid:

### Notification Addiction

TradeForge is not designed to maximize app engagement.

---

### Constant Alerting

Excessive alerts degrade operational cognition.

---

### Emotional Urgency

Avoid emotionally manipulative alert design.

---

### Advisory Authority Leakage

AI attention should not imply autonomous execution authority.

---

### Workflow Destruction

Attention systems should preserve operational continuity.

---

### Metric Obsession

Not every operational metric deserves attention escalation.

---

# Design Invariants

The following remain invariant:

1. Human attention is finite.
2. Operational clarity is more important than stimulation.
3. Calm awareness is preferred over constant interruption.
4. Attention systems must preserve human decision sovereignty.
5. Advisory systems remain non-authoritative.
6. Behavioral awareness should reinforce discipline, not shame.
7. Replayability applies to attention state as well.
8. TradeForge is an operational cognition environment, not an engagement platform.

---

# See Also

* [[DESIGN_ARCHITECTURE]]
* [[workspace-operational-layouts]]
* [[workspace-zoning-reference]]
* [[operational-panel-patterns]]
* [[Human Decision Sovereignty]]
* [[AI Advisory Boundary]]
* [[Replayable Cognition]]
* [[Operating Workspace]]
