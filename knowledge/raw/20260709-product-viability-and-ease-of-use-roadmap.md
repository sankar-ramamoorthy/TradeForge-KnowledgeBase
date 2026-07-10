---
title: Product Viability And Ease-Of-Use Roadmap — Personal/Family Edition
type: roadmap-proposal
status: raw
tags: [tradeforge, product-management, ease-of-use, onboarding, viability, roadmap]
created: 2026-07-09
updated: 2026-07-09
---

# Product Viability And Ease-Of-Use Roadmap — Personal/Family Edition

Derived from [[20260709-comprehensive-product-audit]]. Audience: operator +
family (2–5 people, mixed technical ability). Every item below is scoped so a
Codex/Sonnet-class implementer can execute from a detailed per-issue plan.

North star for this phase:

> A family member can start TradeForge with one command, capture an idea in
> under two minutes, see real evidence arrive without asking for it, paper-
> trade the plan, and get an honest review with actual outcome numbers.

Proposed milestone frame: **M-EZ (Ease Of Use)** and **M-PT (Paper Trading,**
see [[20260709-paper-trading-implementation-plan]]**)**, deliberately ahead of
M15–M18.

---

# Theme 1 — One-Command Start (highest ease-of-use leverage)

## EZ-01: Postgres-by-default single compose stack

- `docker compose up` starts postgres + backend + a served frontend build.
  Today the frontend is dev-server-only and persistence is opt-in.
- Add a frontend build stage to the runtime image (or an nginx sidecar)
  so `http://localhost:8000` serves the app.
- Run `alembic upgrade head` automatically on container start.
- Acceptance: fresh machine with Docker → one command → working persistent
  app in the browser. No uv, no Node, no second terminal.

## EZ-02: In-app first-run wizard (kill the CLI master key step)

- On first boot without `TRADEFORGE_MASTER_KEY` and without `.keys.enc`,
  backend enters "setup mode": UI wizard generates the master key, shows it
  once with copy-to-safe-place instructions, writes it to a compose-mounted
  env file (documented tradeoff vs. OS env var; acceptable at this trust
  tier), and proceeds.
- Credentials remain optional — yfinance default means zero keys needed for
  a working system.
- Acceptance: family member never opens a terminal after `docker compose up`.

## EZ-03: Documentation truth pass

- Fill or delete the empty `DOCS/architecture.md`, `DOCS/domain-model.md`,
  `DOCS/event-schema.md`; rewrite `PROJECT.md` to current state; single
  authoritative roadmap file. Cheap, high value for AI-assisted development.

---

# Theme 2 — Evidence Density (the product's own #1 diagnosis)

The blue-pin questions from the workspace critiques are the spec. Promote
them from raw note to design doc, then implement:

## EV-01: Scheduled market snapshot job

- Background task (in-process scheduler is fine; no new infra) that
  refreshes snapshots for all symbols attached to active decisions +
  watchlist, on a configurable interval (default: hourly during market
  hours, daily close otherwise).
- Snapshots persist through the existing [[MarketContext]] boundary —
  replay-safe, provenance-tagged, no live calls during replay.

## EV-02: Watchlist as a first-class light object

- A watchlist entry is *pre-lifecycle* (not a TradeIdea) — symbol + one-line
  why + created date. Feeds EV-01 scanning and the Opportunity Workspace so
  its "right questions" finally have answers.

## EV-03: Per-symbol evidence panel answering the blue pins

- What is interesting now → last price, % change, volume vs average,
  distance from 52w high/low, next earnings date.
- Confirm/invalidate → simple deterministic levels (prior high/low, moving
  averages) — deterministic first, AI interpretation stays advisory overlay.
- Fundamentals snapshot via existing FMP/Alpha Vantage adapters.
- Every fact carries timestamp + provider provenance (existing pattern).

## EV-04: Basic price chart

- One embeddable candlestick/line chart component fed from persisted
  snapshots/bars. The anti-dashboard doctrine (ADR-0007) opposes dashboard
  *organization*, not visual evidence inside a decision surface. A trader
  product without a single chart fails the family usability test.

---

# Theme 3 — Lower The Entry Ramp (without weakening the gate)

## RAMP-01: Quick-capture idea tier

- "Jot an idea" input: symbol + 2 sentences → creates a TradeIdea with a
  stub thesis marked `draft`. Full structured thesis remains mandatory at
  the Approval gate — rigor moves to where commitment happens, aligned with
  the existing readiness-gate pattern in [[PlanReadinessPanel]] thinking.

## RAMP-02: Guided "first decision" mode for family

- Extend the existing walkthrough/onboarding assets into a persona-neutral
  guided path with plain-language labels (leverage the trader-language
  doctrine, but add a beginner glossary layer).

## RAMP-03: Operator identity (small, not enterprise auth)

- Named operator profiles (no passwords initially; trust tier = household).
  Every canonical event already has an actor slot conceptually — make
  operator_id explicit on new events; filter workspaces by operator.
- This is the minimum for "family" to mean anything; defer real auth.

---

# Theme 4 — Process Calibration (KB/governance)

## GOV-01: Two-tier issue discipline (documented in AGENTS.md/CLAUDE.md)

- Tier A (full ceremony: issue + ADR check + KB capture): anything touching
  domain events, lifecycle, persistence, security, advisory boundary.
- Tier B (batch issue, no per-change ADR pass): frontend copy, layout,
  styling, docs. Cuts solo-operator overhead where it isn't buying safety.

## GOV-02: KB hygiene pass

- Archive the 4.1 MB litellm log out of `knowledge/raw/`; consolidate
  root `raw/` into `knowledge/raw/`; register TF-D001–TF-D008 or mark M15
  explicitly deferred (recommended: deferred).

---

# Sequencing Recommendation

```text
1. M-PT Phase 0–2  (paper trading foundation — see companion plan)
2. EZ-01, EZ-02    (one-command start)        ← do alongside M-PT
3. EV-01 → EV-04   (evidence density)
4. RAMP-01, RAMP-02
5. M-PT Phase 3–5  (position sync + review integration)
6. RAMP-03, GOV-01, GOV-02, EZ-03
```

Rationale: paper trading and one-command start convert TradeForge from
"architecture demo I maintain" into "tool my household actually uses";
evidence density then makes daily use rewarding. Defer M15/M17/M18 until the
loop closes and real usage generates the event history those milestones need
to be interesting.

---

# Explicit Non-Goals (this phase)

- live-money broker execution
- mobile apps
- multi-tenant/hosted deployment
- additional advisory analytics surfaces
- M15 cognitive reconstruction, M17 simulation, M18 adaptive research
