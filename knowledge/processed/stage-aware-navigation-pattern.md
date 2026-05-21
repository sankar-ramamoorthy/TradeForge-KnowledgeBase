---
title: Stage-Aware Navigation Pattern
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0063
source_history:
  - knowledge/raw/20260514-tf-0063-workspace-transition-ergonomics-planning.md
  - knowledge/raw/20260514-tf-0063-workspace-transition-ergonomics-implementation.md
tags:
  - navigation
  - workspace-ergonomics
  - lifecycle
  - ux-architecture
  - stage-aware
---

# Stage-Aware Navigation Pattern

## Purpose

The sidebar workspace navigation marks the contextually appropriate workspace
for the current lifecycle stage. This makes workspace transitions feel
operationally deliberate — the user sees where they should be, rather than
treating all workspaces as equally valid at all times.

## Implementation

### Stage-to-Workspace Map (`workspaceRouting.ts`)

`STAGE_TO_WORKSPACE` is the canonical mapping from lifecycle stage to recommended
`WorkspaceRouteId`. `getRecommendedWorkspace(stage)` returns the mapped id or null.

### Navigation Indicator

`WorkspaceNavigation` accepts `recommendedRouteId?: WorkspaceRouteId | null`.
The recommended link (when not the currently active page) receives:
- CSS class `"recommended"` — accent border + accent surface
- A right-justified `→` indicator span
- Accessible aria-label suffix "— recommended for current stage"

### Derivation in App.tsx

```typescript
const recommendedRouteId = useMemo(
  () => getRecommendedWorkspace(activeStage),
  [activeStage],
);
```

`activeStage` is initialized from persisted `last_known_stage` (TF-0062),
so the recommendation is correct immediately after page refresh.

## Design Principles

- Advisory only — no nav link is hidden or disabled; recommendation is visual guidance
- Never marks the currently active workspace (no redundant "you're already here" signal)
- Operating workspace is never in the recommendation map — it's the universal hub,
  always accessible regardless of stage
- Consistent with TF-0057's post-transition routing (both derived from the same stage→workspace logic)

## Extension Point

To change which workspace is recommended for a stage, update `STAGE_TO_WORKSPACE`.
The indicator, accessibility labels, and CSS classes follow automatically.
