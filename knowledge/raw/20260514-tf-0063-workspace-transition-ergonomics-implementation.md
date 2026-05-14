---
title: TF-0063 Implementation Capture — Workspace Transition Ergonomics
date: 2026-05-14
status: raw
type: implementation-capture
issue: TF-0063
milestone: M10
---

# TF-0063 Implementation Capture

## Summary

Added a stage-aware recommended workspace indicator to the sidebar nav.
When a lifecycle stage is active, the nav highlights the contextually appropriate
workspace with an accent border and "→" indicator. No nav links are removed or
hidden — the recommendation is advisory, not restrictive.

## Files Changed

### `frontend/src/workspaceRouting.ts`

Added:
- `STAGE_TO_WORKSPACE: Partial<Record<string, WorkspaceRouteId>>` — canonical mapping
  of each lifecycle stage to its recommended workspace
- `getRecommendedWorkspace(stage)` — returns the recommended WorkspaceRouteId or null

Mapping:
```
Idea     → opportunity
Thesis   → plan-review
Plan     → plan-review
Approval → plan-review
Execution → active-position
Position → active-position
Review   → review
```
(Operating is always accessible as the hub; it maps from no stage, not from a specific stage)

### `frontend/src/operationalLayout.tsx`

`WorkspaceNavigation` gains optional prop `recommendedRouteId?: WorkspaceRouteId | null`.

Logic: `isRecommended = !isActive && route.id === recommendedRouteId`
(never marks the currently active workspace as recommended — user is already there)

Changes to the rendered `<a>`:
- className includes `"recommended"` when isRecommended
- `aria-label` adds "— recommended for current stage" for accessibility
- Conditionally renders `<span className="workspace-nav-recommended-indicator">→</span>`
  with `margin-left: auto` to push it to the far right of the link

### `frontend/src/App.tsx`

- Import: `getRecommendedWorkspace`
- `recommendedRouteId` = `useMemo(() => getRecommendedWorkspace(activeStage), [activeStage])`
- Passed as `recommendedRouteId={recommendedRouteId}` to `WorkspaceNavigation`

### `frontend/src/styles.css`

```css
.workspace-nav a.recommended {
  border-color: var(--color-accent);
  background: var(--color-accent-surface);
}

.workspace-nav-recommended-indicator {
  margin-left: auto;
  color: var(--color-accent);
  font-size: 0.9rem;
  font-weight: 700;
}
```

## User Experience Before/After

Before (at Plan stage, in Operating workspace):
```
● Operating  ← active, accent border
  Opportunity
  Plan Review
  Active Position
  Replay
  Review
```

After (at Plan stage, in Operating workspace):
```
● Operating      ← active (dark green)
  Opportunity
  Plan Review →  ← recommended (accent border + "→")
  Active Position
  Replay
  Review
```

The user immediately sees where their current stage belongs.

## Tests

- `npm.cmd run typecheck` — clean
- `npm.cmd run build` — clean (268.18 kB JS, 29.35 kB CSS)
- `uv run pytest` — 513 passed (no backend changes)

## Operational Lessons

- `isRecommended = !isActive && ...` ensures the marker only appears when the recommended
  workspace differs from the current page. No redundant "you should be here and you are here"
  visual noise.
- `STAGE_TO_WORKSPACE` is separate from the workflow transition routing in TF-0057 — that
  one handles post-action navigation, this one handles sidebar guidance. They're consistent
  but independent.
- The `useMemo` on `recommendedRouteId` (keyed on `activeStage`) means it recomputes only
  when the stage changes, not on every render.
- `activeStage` is now initialized from `getOperationalContext().last_known_stage` (TF-0062),
  so the recommendation is correct immediately after a page refresh.
