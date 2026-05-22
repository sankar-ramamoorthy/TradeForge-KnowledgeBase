---
title: TF-0063 Planning Capture — Workspace Transition Ergonomics
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0063
milestone: M10
---

# TF-0063 Planning Capture

## Issue Goal

Workspace transitions feel operationally deliberate rather than technical.

Scope: navigation flow, transition clarity, workspace movement behavior,
contextual navigation stability.

## Identified Problem

The sidebar nav shows all 6 workspaces as equal options with no indication
of which is contextually appropriate for the current lifecycle stage.

A user at "Plan" stage sees six identical-looking nav links and must
already know that "Plan Review" is the right workspace. The nav is
workspace-agnostic — it provides no operational guidance.

Result: transitions feel like arbitrary tab-switching, not deliberate
workflow movement.

## Design

### Stage-to-Workspace Mapping

Add `STAGE_TO_WORKSPACE` canonical map to `workspaceRouting.ts`:

```
Idea     → opportunity
Thesis   → plan-review
Plan     → plan-review
Approval → plan-review
Execution → active-position
Position → active-position
Review   → review
```

Operating is always the hub — never "recommended" from a stage perspective.
It's always accessible as the attention hub.

### Recommended Workspace Indicator in Nav

When a lifecycle stage is active:
- The workspace matching the current stage gets an `a.recommended` CSS class
- Visual: accent border + accent surface background + "→" indicator pushed right
- Only applied when the recommended workspace is NOT the currently active page
  (no point marking where you already are)

### WorkspaceNavigation prop update

`WorkspaceNavigation` gains optional `recommendedRouteId?: WorkspaceRouteId | null`.
App.tsx derives it from `activeStage` via `getRecommendedWorkspace(activeStage)`.

### Layout: no changes needed

Current nav link is `display: flex; align-items: center; gap: 10px`.
Adding a third `span.workspace-nav-recommended-indicator` with `margin-left: auto`
pushes it to the right without any layout restructuring.

## Files to Change

1. `workspaceRouting.ts` — add `STAGE_TO_WORKSPACE`, `getRecommendedWorkspace()`
2. `operationalLayout.tsx` — update `WorkspaceNavigation` props, add indicator span
3. `App.tsx` — derive `recommendedRouteId`, pass to `WorkspaceNavigation`
4. `styles.css` — `.workspace-nav a.recommended` + `.workspace-nav-recommended-indicator`

## Invariants

- The recommended indicator is advisory/display — does not enforce navigation
- User can freely navigate to any workspace regardless of recommendation
- No new API calls or lifecycle state changes

## Expected Outcome

At Plan stage, user sees:
```
  Operating
  Opportunity
● Plan Review  →     ← accent border + "→" indicator
  Active Position
  Replay
  Review
```

The workspace for their current stage is visually distinct.
Transitions feel directed, not accidental.
