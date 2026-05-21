---
title: TF-0060 Implementation Capture — One-Click Operational Walkthrough
date: 2026-05-14
status: raw
type: implementation-capture
issue: TF-0060
milestone: M10
---

# TF-0060 Implementation Capture

## Summary

Implemented a 7-step guided walkthrough that progresses through all lifecycle stages
with contextual explanation at each workspace. One click to start; one click per step to advance.

## Files Changed

### New files

- `frontend/src/walkthrough.ts`
  - `WalkthroughStepDef` type (7 steps, 0-indexed)
  - `WalkthroughSession` type (localStorage-persisted)
  - `WALKTHROUGH_STEPS` constant (7 steps covering all lifecycle stages)
  - `getWalkthroughSession` / `setWalkthroughSession` / `clearWalkthroughSession`
  - `initWalkthrough(options)` — creates AAPL Idea decision, activates it, stores session
  - `advanceWalkthroughStep(session, step)` — fires sequential lifecycle transitions

- `frontend/src/workspaces/WalkthroughPanel.tsx`
  - Persistent `<aside>` panel rendered from App.tsx above workspace content
  - Shows: step dot indicator, step title, explanation, Continue/Finish button, Exit

### Modified files

- `frontend/src/App.tsx`
  - Imports: walkthrough module + WalkthroughPanel
  - State: `walkthroughSession`, `walkthroughAdvancing`, `walkthroughError`
  - `handleStartWalkthrough()` — async, calls initWalkthrough, sets state, navigates to step 0
  - `handleWalkthroughAdvance()` — fires lifecycle transitions, advances step, navigates
  - `handleExitWalkthrough()` — clears session
  - `handleClearDecision()` — now also clears walkthrough session
  - Render: WalkthroughPanel injected as first child of WorkspaceLayout's children (above workspace)
  - `onStartWalkthrough` prop passed to OperatingWorkspace

- `frontend/src/workspaces/OperatingWorkspace.tsx`
  - New prop: `onStartWalkthrough?: () => Promise<void>`
  - New state: `walkthroughStarting`, `walkthroughStartError`
  - New UI section: below scenario grid — divider + "Prefer a step-by-step guided tour?" + "Start Guided Walkthrough →" button

- `frontend/src/styles.css`
  - `.demo-section-divider`, `.walkthrough-invite`, `.walkthrough-invite-btn`
  - `.walkthrough-panel`, `.walkthrough-header`, `.walkthrough-dots`, `.walkthrough-dot`
  - `.walkthrough-label`, `.walkthrough-exit`, `.walkthrough-body`
  - `.walkthrough-step-title`, `.walkthrough-explanation`, `.walkthrough-error`
  - `.walkthrough-footer`, `.walkthrough-advance`, `.walkthrough-finish`

## 7-Step Walkthrough Design

| Step | Workspace | Stage Transitions | Explanation Focus |
|------|-----------|------------------|-------------------|
| 0 | Operating | → Thesis | Operating Workspace as attention hub |
| 1 | Opportunity | → Plan | Thesis as structured reasoning |
| 2 | Plan Review | → Approval | Explicit plan before authorization |
| 3 | Plan Review | → Execution + Position | Authorization as immutable commitment |
| 4 | Active Position | → Review | Thesis integrity monitoring |
| 5 | Review | (none) | Process vs. outcome separation |
| 6 | Replay | (none — exit) | Immutable auditable decision history |

Step 3 fires two transitions (Execution + Position) in one click — combining them because
both happen before the active position management phase and separating them adds no educational value.

## Session Persistence

WalkthroughSession stored as `tradeforge.walkthrough_session` in localStorage.
Restored on page refresh. If user refreshes mid-walkthrough, panel resumes at last step.
`handleClearDecision()` now clears both active decision and walkthrough session together.

## Tests Executed

- `npm.cmd run typecheck` — clean (fixed `() => Promise<void>` prop type mismatch)
- `npm.cmd run build` — clean (263.03 kB JS, 26.83 kB CSS)
- `uv run pytest` — 513 passed

## Operational Lessons

- Injecting WalkthroughPanel as first child of WorkspaceLayout's workspace-main grid
  (from App.tsx) keeps all workspace components walkthrough-unaware. Clean separation.
- The `onStartWalkthrough: () => Promise<void>` prop design lets OperatingWorkspace manage
  its own loading state while the actual async work (initWalkthrough) happens in App.tsx.
- Step 6's `nextWorkspacePath: null` is the sentinel that triggers exit rather than advance.
  This is cleaner than a separate `isLastStep` flag.
- Clearing the walkthrough session when clearing the active decision prevents orphaned
  walkthrough state if the user manually clears their decision mid-walkthrough.
