---
title: TF-0061 Implementation Capture — Operational Onboarding Flow
date: 2026-05-14
status: raw
type: implementation-capture
issue: TF-0061
milestone: M10
---

# TF-0061 Implementation Capture

## Summary

Implemented a 5-screen philosophical onboarding modal shown on first visit (localStorage flag).
No API calls, no lifecycle events — purely informational. Teaches WHY before HOW.

## Files Changed

### New files

- `frontend/src/onboarding.ts`
  - `isOnboardingComplete()` — checks `localStorage["tradeforge.onboarding_complete"]`
  - `markOnboardingComplete()` — sets the flag

- `frontend/src/workspaces/OnboardingModal.tsx`
  - 5 `OnboardingScreen` definitions with icon, headline, body
  - `OnboardingModal` component: full-screen overlay, centered card
  - Navigation: Previous / Next buttons (screens 1-4), "Get Started →" (screen 5)
  - "Skip" button (top-right) calls `onComplete` immediately
  - Dot progress indicator

### Modified files

- `frontend/src/App.tsx`
  - Imports: `onboarding.ts`, `OnboardingModal`
  - State: `onboardingDone` (initialized from `isOnboardingComplete()`)
  - Handler: `handleOnboardingComplete()` — calls `markOnboardingComplete()`, sets state
  - Render: `<>` fragment wrapping `OnboardingModal` + `<AppShell>`
  - OnboardingModal renders before AppShell when `!onboardingDone`

- `frontend/src/styles.css`
  - `.onboarding-overlay` (fixed full-screen, dark backdrop, z-index 1000)
  - `.onboarding-card` (centered, max-width 480px, padded)
  - `.onboarding-skip` (absolute positioned top-right button)
  - `.onboarding-icon-wrap` / `.onboarding-icon` (accent circle with icon)
  - `.onboarding-headline` / `.onboarding-body` (centered text)
  - `.onboarding-dots` / `.onboarding-dot` with `.ob-done` / `.ob-current`
  - `.onboarding-footer` / `.onboarding-prev` / `.onboarding-next` / `.onboarding-finish`

## 5 Screens

| ID | Icon | Headline | Principle |
|----|------|----------|-----------|
| sovereignty | Compass | "A decision system, not a trading bot." | Human decision sovereignty |
| lifecycle | GitBranch | "Every trade follows a structured path." | Canonical lifecycle |
| workspaces | Layout | "Workspaces are not dashboards." | Cognitive environments |
| review | BookOpen | "The review is as important as the trade." | Review-first philosophy |
| replay | History | "Every decision is permanently recorded." | Replayability |

## Trigger Logic

Shown when `localStorage["tradeforge.onboarding_complete"] !== "true"`.
Dismissed by: completing all 5 screens ("Get Started") or clicking "Skip".
Once dismissed, never shown again (flag persists in localStorage).

## Tests Executed

- `npm.cmd run typecheck` — clean
- `npm.cmd run build` — clean (266.45 kB JS, 29.13 kB CSS)
- `uv run pytest` — 513 passed

## Operational Notes

- The React fragment wrapper in App.tsx (`<>...</>`) is required because the
  OnboardingModal sibling renders at the same level as AppShell.
- The modal has `role="dialog"` and `aria-modal="true"` for accessibility.
- `position: fixed; inset: 0` on overlay covers the full viewport regardless of scroll position.
- Using `z-index: 1000` ensures it overlays the WalkthroughPanel (if somehow both active).
- The "Get Started →" button on the last screen uses `.onboarding-finish` (darker background)
  to visually signal finality vs. the accent-colored "Next →" flow.
