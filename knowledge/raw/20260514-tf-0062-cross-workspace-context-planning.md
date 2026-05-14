---
title: TF-0062 Planning Capture — Cross-Workspace Context Persistence
date: 2026-05-14
status: raw
type: planning-capture
issue: TF-0062
milestone: M10
---

# TF-0062 Planning Capture

## Issue Goal

Workspace transitions preserve operational meaning.

Scope: shared operational state, cross-workspace context memory,
synchronized decision context, operational continuity services.

## Identified Friction Points

1. MarketContextPanel (OpportunityWorkspace, ActivePositionWorkspace): symbol input resets
   to empty on every workspace navigation. User retypes the same symbol repeatedly.

2. ContextualBriefingPanel (OperatingWorkspace): symbol input resets to empty on navigation.

3. The sidebar `activeStage` badge is derived from `onStageLoaded` callbacks which fire
   only when the workspace projection loads. Between navigation and load, the badge briefly
   shows null/stale stage. No persistence across page refreshes.

## Design Decision: localStorage-backed OperationalContext

Store cross-workspace operational state in localStorage under `tradeforge.operational_context`.
Panels read from this store on mount (no prop drilling needed).
App.tsx syncs active decision symbol and stage into the store.

### OperationalContext shape

```typescript
type OperationalContext = {
  watched_symbols: string[];    // symbols the user cares about in this session
  last_known_stage: string | null;  // last lifecycle stage seen, for badge persistence
};
```

### Key functions

- `syncDecisionSymbol(symbol)` — when active decision changes, put its symbol at front of watched list
- `addWatchedSymbols(symbols)` — called after a market fetch, adds any new symbols used
- `getWatchedSymbolsString()` — comma-joined string for pre-filling input fields
- `syncLastKnownStage(stage)` — called by onStageLoaded callback, persists for badge init

### No prop drilling

All three panels (MarketContext, ContextualBriefing) read from localStorage directly.
No changes to workspace prop interfaces.
App.tsx useEffect syncs active decision symbol. handleStageLoaded also calls syncLastKnownStage.

## Architecture

### New file: operationalContext.ts
Storage key: `tradeforge.operational_context`
Pure functions: get, set, sync, add — no React state, just localStorage.

### Modified: MarketContextPanel.tsx
- useState initial value: `() => getWatchedSymbolsString()` instead of `""`
- After successful fetch: call `addWatchedSymbols(symbols)`

### Modified: ContextualBriefingPanel.tsx
- Same pattern as MarketContextPanel

### Modified: App.tsx
- useEffect: when `activeDecision?.symbol` changes → `syncDecisionSymbol(symbol)`
- Initialize `activeStage` from `getOperationalContext().last_known_stage` instead of null
- In handleStageLoaded: also call `syncLastKnownStage(stage)`

### Modified: styles.css
No significant changes needed.

## Invariants

- Watched symbols are advisory metadata — never written to the event ledger
- last_known_stage is derived from projection data — display hint only, not canonical truth
- All persistence is session-local (localStorage), not server-side

## Expected Outcome

After TF-0062:
- User starts an AAPL trade idea → MarketContextPanel auto-fills "AAPL" on every workspace visit
- User fetches "AAPL,SPY" → both symbols remembered for next panel visit
- Sidebar stage badge shows last known stage immediately on workspace transition (no null flash)
- Page refresh restores last known stage in badge and symbols in panels

## Tradeoffs

- Watched symbols accumulate over sessions unless cleared. Could become stale if user works
  on multiple decisions. Acceptable for MVP — user can manually retype to override.
- `getWatchedSymbolsString()` runs at component mount — if the value is empty and an
  active decision exists, the App.tsx useEffect may not have run yet. This is acceptable:
  the symbol sync fires after mount, and on the next workspace visit the input will be
  pre-filled correctly.
- Clearing the active decision does NOT clear watched symbols — these are separate concerns.
  The user may still want to see the same symbols after clearing a decision.
