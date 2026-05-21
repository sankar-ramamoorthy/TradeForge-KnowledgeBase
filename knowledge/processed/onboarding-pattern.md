---
title: Philosophical Onboarding Pattern
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0061
source_history:
  - knowledge/raw/20260514-tf-0061-onboarding-flow-planning.md
  - knowledge/raw/20260514-tf-0061-onboarding-flow-implementation.md
tags:
  - onboarding
  - demoability
  - ux-architecture
  - philosophy
  - first-visit
---

# Philosophical Onboarding Pattern

## Purpose

TradeForge's onboarding teaches the system's philosophical principles before
allowing operational interaction. This is distinct from functional tutorials
(TF-0060 walkthrough) — it establishes the WHY before the HOW.

## Architecture

### Trigger

`localStorage["tradeforge.onboarding_complete"] === "true"` gates display.
Shown once on first visit; never shown again after completion or skip.

### Component Structure

`OnboardingModal` is rendered in App.tsx BEFORE `AppShell`, wrapped in a React fragment.
The `position: fixed; inset: 0` overlay covers the entire viewport with `z-index: 1000`.

This placement ensures:
- The modal overlays the entire application
- No workspace content is accessible until dismissed
- The fragment wrapper is minimal — no container changes needed

### Screen Content Principles

Each screen covers one invariant from `INVARIANTS.md`:
1. Human decision sovereignty
2. Lifecycle authority (Idea → Review, no skipping)
3. Workspaces as cognitive environments (anti-dashboard)
4. Review as first-class workflow
5. Replayability as foundational

Content is philosophical, not instructional. No UI directions.
3–4 sentences per screen. Centered layout. Icon as visual anchor.

## Relationship to Other Demoability Features

```
Onboarding (TF-0061): WHY — philosophical context, first visit only
Walkthrough (TF-0060): HOW — step-by-step through lifecycle, on demand
Scenarios (TF-0059): WHAT — pre-seeded examples to explore, on demand
```

All three are complementary. Onboarding sets the mental model; walkthrough and scenarios
provide operational experience.

## Storage Key

`tradeforge.onboarding_complete` — separate from walkthrough and active decision storage.
