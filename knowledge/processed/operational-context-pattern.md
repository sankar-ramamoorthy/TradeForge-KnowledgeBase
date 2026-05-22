---
title: Operational Context Pattern
status: processed
type: processed-knowledge
created: 2026-05-14
updated: 2026-05-14
source_issues:
  - TF-0062
source_history:
  - knowledge/raw/20260514-tf-0062-cross-workspace-context-planning.md
  - knowledge/raw/20260514-tf-0062-cross-workspace-context-implementation.md
tags:
  - operational-context
  - cross-workspace
  - localStorage
  - continuity
  - frontend
---

# Operational Context Pattern

## Purpose

`OperationalContext` is a lightweight localStorage store (`tradeforge.operational_context`)
that persists cross-workspace state that is too transient for the active decision record
but too important to lose on every navigation.

## Shape

```typescript
type OperationalContext = {
  watched_symbols: string[];      // symbols the user cares about this session
  last_known_stage: string | null; // most recent lifecycle stage — for badge init
};
```

## Sync Points

| Event | Operation |
|-------|-----------|
| Active decision changes | `syncDecisionSymbol(symbol)` — puts symbol at front |
| Market context panel fetches | `addWatchedSymbols(symbols)` — appends new ones |
| Any workspace reports its stage | `syncLastKnownStage(stage)` — persists for badge init |
| Active decision cleared | `clearOperationalContext()` — full reset |

## Design Principle: No Prop Drilling

Components read from `operationalContext.ts` directly at mount time via lazy `useState` initializers.
App.tsx writes to it via useEffect and callback enhancements.
No new props added to any workspace component interface.

This keeps workspace components operationally context-aware without coupling them to
App.tsx's state shape.

## Consumer Pattern

```typescript
// In a component — auto-populate input on mount
const [input, setInput] = useState(() => getWatchedSymbolsString());

// After a successful fetch — remember the symbols
addWatchedSymbols(symbols);
```

## Lifecycle

Created implicitly on first write. Cleared when active decision is cleared.
Persists across page refreshes (localStorage). Does not persist across server restarts
(localStorage is browser-local, not server-side).

## Invariant Compliance

- `watched_symbols` are advisory metadata — never written to event ledger
- `last_known_stage` is a display hint derived from workspace projections — not canonical truth
- All state is session-local, non-authoritative

## Related

- [[demo-scenario-architecture]] — scenarios auto-trigger syncDecisionSymbol via activeDecision
- [[guided-walkthrough-pattern]] — walkthrough decisions also trigger syncDecisionSymbol
