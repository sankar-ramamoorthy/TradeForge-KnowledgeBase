---
title: Comprehensive Product Audit — Runtime And Knowledge Base
type: audit
status: raw
tags: [tradeforge, audit, product-management, viability, runtime, knowledge-base]
created: 2026-07-09
updated: 2026-07-09
---

# Comprehensive Product Audit — Runtime And Knowledge Base

## Trigger

Operator requested a comprehensive audit of both repositories with a product
manager lens. Audience: the operator and possibly family. Goals: viability,
ease of use, paper trading. Analysis only — no code changes in this pass.

Companion notes:

- [[20260709-product-viability-and-ease-of-use-roadmap]]
- [[20260709-paper-trading-implementation-plan]]

---

# Part 1 — What Is Genuinely Strong

## Architecture discipline is exceptional for a solo project

- Event-sourced core with strict lifecycle
  (`Idea → Thesis → Plan → Approval → Execution → Position → Review`),
  enforced by [[LifecycleTransitionValidator]] and covered by 81 test modules
  (700+ tests), ruff, and strict mypy.
- 33+ ADRs with real rationale, a maintained `ISSUE_REGISTER.md`
  (~120 issues, nearly all Done through M14), and a KB synthesis loop
  (raw → processed → entities) that most professional teams never achieve.
- Clean layer separation (`domain` / `services` / `infrastructure` /
  `security` / `app`) that matches the documented rules — verified by
  inspection, not just claimed.
- The credential boundary (encrypted `.keys.enc`, master key, masked UI,
  per-request LiteLLM secret injection per M13B) is thoughtfully designed.

## The product concept is differentiated

The most valuable sentence in the entire KB (from
`raw/feedback-on-the-current-status…`):

> Organized around questions, not data. Many platforms help traders consume
> information. TradeForge is trying to help traders think.

This is a real differentiator. Nothing mainstream does structured
pre-commitment ("write down why before you act, and be held to it") plus
honest replay.

## Governance of AI is ahead of the market

The advisory-only boundary, provenance tracking, authority labeling, and the
[[Ask-David-Pattern]] adaptation (advisory council, not autonomous brain) are
the right shape for 2026-era AI products.

---

# Part 2 — Audit Findings (Runtime)

## F1. The product models cognition better than evidence (critical)

Your own diagnosis, and the audit confirms it. Workspaces ask excellent
questions (What confirms this? What invalidates it?) but the evidence supply
is thin: manual Context Workbench acquisition, single-symbol snapshots, no
background refresh, no watchlist scanning, no charts. The framework is ahead
of the facts. This is the #1 product gap.

## F2. The lifecycle loop never closes without manual data entry (critical)

Execution and Position stages are manual recordings. No broker link means:

- fills, prices, and outcomes are typed in by hand
- Review quality analysis rests on hand-entered numbers
- the Armed state (conditional execution) requires the operator to watch
  the market themselves — the system cannot notice a trigger condition

Paper trading directly fixes this. See
[[20260709-paper-trading-implementation-plan]].

## F3. Onboarding friction is far above the target audience's tolerance

Current first-run: install uv + Python + Node, two terminals, master key via
CLI + OS environment variable, credentials via CLI or governance UI, Docker
Compose for persistence, LiteLLM profile for advisory. Fine for you; a wall
for family. Persistence should be the default experience, not the advanced
path (in-memory default means a family member's first decision evaporates on
restart).

## F4. Authoring burden is heavy

A structured thesis requires narrative, catalysts, assumptions, invalidation
conditions, confidence, regime alignment — before a plan can even start.
That rigor is the product's soul, but there is no "quick capture" tier. The
M14C import bridge (TF-R001/R002) helps the Codex-cockpit user, not the
in-app family user.

## F5. Single-operator assumption

Session model exists (ADR-0022) but there is no concept of separate operators
with separate decision histories. "Me and family" means 2–5 operators.
Personas are interpretation lenses, not identities — this needs a small,
explicit identity layer eventually.

## F6. Maintenance risks that will bite lower-capability coding models

- `src/app/api/routes.py` is a 7,504-line monolith holding 82 routes.
  Response-model classes, wiring, and handlers all live there. A Sonnet-class
  implementer working from a plan will make collateral edits in a file this
  size. Split it by router before commissioning major new work.
- `frontend/src/api/runtime.ts` likely mirrors this scale (single API client).
- Stale/empty docs: `DOCS/architecture.md`, `DOCS/domain-model.md`,
  `DOCS/event-schema.md` are 0 bytes; `PROJECT.md` still describes the
  pre-M2 foundation phase while M14 is complete. Stale truth is worse than
  absent truth for AI-assisted development.
- M15 issues TF-D001–TF-D008 exist in the roadmap but not the issue register
  (known gap, flagged in the M15 brainstorm note).

## F7. No scheduled/background anything

All market context is pull-on-demand. No daily snapshot job, no evidence
freshness sweep, no alerting. Both the evidence gap (F1) and Armed-state
watching (F2) trace back to this.

---

# Part 3 — Audit Findings (Knowledge Base)

## K1. The KB works, but curation lags

- `knowledge/processed/` (190+ syntheses) and `knowledge/raw/archived/` are
  healthy. But a 4.1 MB `logs from litellm container.txt` sits in raw
  (should be archived or deleted), and there are two parallel raw locations
  (`raw/` at repo root and `knowledge/raw/`) — consolidate.
- Roadmap authority is split: v3 is marked historical, v2 is authority, and a
  deprecated v1 also remains in runtime DOCS. One authoritative roadmap file
  would prevent AI-session confusion.

## K2. Governance ceremony is calibrated for a team, priced to a solo operator

Issue discipline + ADR checkpoints + KB capture per issue is the reason
quality is high — keep it for domain/event/lifecycle changes. But UX polish,
copy changes, and small frontend fixes pay the same process tax. Recommend a
documented two-tier discipline: full ceremony for canonical-truth changes,
lightweight batch issues for surface changes.

## K3. The KB's best thinking is not yet product-facing

The blue-pins evidence requirements ("What is interesting now? → catalyst,
price, volume, recent change…") are precise product specs sitting in a raw
note. They should be promoted to a design doc that directly drives the
evidence-density milestone.

---

# Part 4 — Product Manager Verdict

For the stated audience (you + family), TradeForge is architecturally
over-built and product-under-delivered — which is the *right* failure mode,
because architecture is hard to retrofit and product is iterable. The
foundation is sound; do not redesign it.

The viability equation for a 2–5 person audience:

```text
Viable = (evidence arrives by itself)
       + (outcomes record themselves)      ← paper trading
       + (starting takes one command)
       + (a decision can start small)
```

Everything else — cognitive replay (M15), simulation (M17), adaptive research
(M18) — is genuinely interesting and should wait. Those milestones deepen a
loop that does not yet close on its own.

Recommended priority order (detail in
[[20260709-product-viability-and-ease-of-use-roadmap]]):

1. **Paper trading via Alpaca paper API** — closes the lifecycle loop,
   makes Review honest, zero financial risk, and `alpaca-py` is already a
   dependency. See [[20260709-paper-trading-implementation-plan]].
2. **Evidence density** — background snapshots, watchlists, richer
   per-symbol evidence panels answering the blue-pin questions.
3. **One-command start + first-run wizard** — Postgres-by-default compose,
   master key generated in-app on first boot, credentials optional.
4. **Quick-capture idea tier** — lower the entry ramp without weakening the
   approval gate.
5. **Operator identity** — small multi-user layer for family.

Deliberately deferred: M15/M17/M18, mobile apps, additional advisory
analytics, more provider governance surface.
