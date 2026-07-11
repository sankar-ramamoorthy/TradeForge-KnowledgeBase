---
title: Multi-User Edition, Day Trading Persona, And Cloud Strategy
type: strategy-brainstorm
status: raw
tags: [tradeforge, product-strategy, distribution, day-trading, persona, cloud, gcp, aws, azure, career]
created: 2026-07-09
updated: 2026-07-09
---

# Multi-User Edition, Day Trading Persona, And Cloud Strategy

Operator question: build a second app from TradeForge for a larger audience,
distributed as "each user downloads a copy"; add a day trading persona;
evaluate web-based delivery; use the work to demonstrate cloud
(GCP/AWS/Azure) experience for career growth.

Related: [[20260709-comprehensive-product-audit]],
[[20260709-product-viability-and-ease-of-use-roadmap]],
[[20260709-paper-trading-implementation-plan]].

---

# 1. Do NOT fork — build editions from one codebase

The instinct "create another app based on this app" should be resisted in
its literal form. A fork doubles maintenance forever, and the KB/ADR/issue
governance that makes this project exceptional cannot follow two diverging
runtimes. A solo operator + AI coding agents cannot sustain two products.

Instead: **one runtime, multiple editions**, differing only in:

- deployment profile (local single-user / downloadable / hosted)
- persona packs (swing/investor today; day trading as a new pack)
- feature flags (e.g., advisory on/off, paper-only vs. future live)

The architecture is unusually well-positioned for this: the ports/adapters
discipline (EventStorePort, provider registry, credential boundary) means
editions are composition-root configurations, not code branches. Divergence
must be config, never copy-paste.

# 2. The "everyone downloads a copy" model — what it takes

Current install reality: uv + Node + two terminals, or Docker Compose.
Acceptable for developers; a wall for a general audience.

## Tier D1 — Docker distribution (cheap, near-term)

- Prereq: EZ-01/EZ-02 (one-command compose, first-run wizard) from the
  ease-of-use roadmap.
- Audience: technical traders. Effort: small on top of existing plans.

## Tier D2 — Single executable ("download, double-click, browser opens")

This is the realistic consumer-local target and a genuine milestone:

- **SQLite event store adapter** — Postgres is too heavy to bundle.
  EventStorePort already has in-memory and Postgres implementations, so
  this is well-bounded work; alembic supports SQLite with care around
  ALTER TABLE limitations. Advisory/snapshot stores need the same
  treatment.
- Package backend + built frontend into one binary (PyInstaller or
  briefcase; frontend served as static files by FastAPI — also planned in
  EZ-01).
- First-run wizard (EZ-02) becomes mandatory, not optional.
- **Auto-update** mechanism + versioned event-schema migrations that never
  corrupt a user's ledger (event sourcing helps: append-only data ages
  well).
- Code signing certificates (Windows + macOS notarization) — annual cost
  and process, or users face scary security warnings.
- Crash reporting / opt-in telemetry, without which remote support of
  strangers' machines is guesswork.

## The honest costs of local distribution at scale

- Support burden lands on every user's unique machine.
- No usage visibility; updates adopted slowly; old versions linger.
- Weak career story: no cloud infrastructure involved.
- Strong privacy story: broker/LLM keys never leave the user's machine —
  this matches the current credential architecture perfectly and is the
  model's main virtue.

# 3. Day trading persona — bigger than a persona

The Persona invariant (interpretation only, no state mutation) means the
*formal* persona part is cheap: different attention weighting, regime
interpretation, workspace summaries. The real work is what day trading
demands from the platform:

## 3a. Lifecycle ergonomics (the philosophical challenge)

`Idea → Thesis → Plan → Approval → Execution → Position → Review` is
deliberately slow; day traders decide in seconds. The lifecycle must NOT be
collapsed (invariant #2). The resolution is **playbook-based
pre-commitment**:

- Operator authors a [[Playbook]] per setup (e.g., opening-range breakout)
  once: thesis template, entry/stop/size rules, invalidation. This is a
  morning/weekend activity done at thesis-quality depth.
- Intraday, a trade is a lightweight instantiation referencing the
  playbook: the thesis/plan artifacts are templated from it in seconds,
  and Approval means "conforms to an approved playbook."
- Review gains a **per-session review** artifact (end of day) alongside
  per-decision review.
- This is philosophically *stronger* than normal day trading apps: the
  discipline system (playbook adherence tracking, M14 behavioral signals —
  impulsive-execution detection already exists as a concept) is more
  valuable for day traders than for swing traders. The pitch: "the
  discipline layer day trading platforms don't have."
- Existing playbook-alignment projections (M10AIS14, PlaybookAlignmentPanel)
  are the seed of this.

## 3b. Intraday data (the expensive part)

- Current model: on-demand snapshots + planned hourly refresh (EV-01).
  Day trading needs 1–5 minute bars and near-real-time quotes → Alpaca or
  Polygon websocket streaming, a streaming ingestion service, and
  replay-safe persistence of intraday snapshots (event-sourcing storage
  volume grows sharply; needs retention/rollup design).
- Paper trading (M-PT) is a hard prerequisite — day trading without
  execution feedback is pointless, and Alpaca paper supports intraday.

## 3c. Sizing

Persona pack + playbook lifecycle + session review: comparable to M-PT in
scope. Streaming intraday data layer: another similar-sized milestone.
Sequence AFTER M-PT and the evidence work — the dependencies literally
stack (paper trading → intraday data → day trading persona).

# 4. Local-download vs. web-based — recommendation

## The decision is not binary; it's a staged path

The same codebase supports both. But the strategic recommendation, given
BOTH the larger-audience goal and the career goal:

```text
Stage 0  (now)      Finish the loop: M-PT, EZ, EV — product worth sharing
Stage 1  (career)   Deploy YOUR OWN single-user instance to one cloud
Stage 2  (audience) D1/D2 downloadable edition for early users (paper-only)
Stage 3  (scale)    Multi-tenant hosted SaaS — only if real demand appears
```

## Stage 1 is the highest career-value-per-effort move

Deploying the existing single-user app to a cloud, properly, produces
nearly all the résumé value with none of the multi-tenant risk (no other
people's data):

- Container → Cloud Run (GCP) or ECS/Fargate (AWS) or Container Apps
  (Azure)
- Postgres → Cloud SQL / RDS / Azure Database for PostgreSQL
- Master key + provider secrets → **KMS + Secret Manager** (replacing the
  local env-var master key with cloud KMS envelope encryption is a
  genuinely impressive, well-scoped project — it extends ADR-0037's
  credential boundary rather than discarding it)
- CI/CD (GitHub Actions → build, test, deploy), IaC with **Terraform**,
  structured logging + monitoring/alerts, custom domain + TLS + IAP or
  equivalent auth gate.

Career advice embedded here: do NOT build on three clouds. Pick ONE
(GCP Cloud Run is the cheapest/simplest for this shape; AWS has the larger
job market — choose by target jobs), go deep, and use Terraform so the IaC
skill reads as transferable. A written architecture doc + diagrams + a live
URL beats checkbox multi-cloud every time. A second cloud can be added
later as a Terraform re-target exercise (itself a good story).

## What full multi-tenant SaaS additionally requires (Stage 3, eyes open)

- Real authentication/authorization (ADR-0022 is currently a local session
  stub) — OIDC (Auth0/Firebase Auth/Cognito), operator identity on every
  event (aligns with RAMP-03).
- Tenant isolation in the event store and all projections/stores
  (tenant_id scoping everywhere; row-level security or schema-per-tenant).
- Per-tenant credential encryption via KMS; you become custodian of users'
  broker + LLM keys — the responsibility step-change is enormous.
- Uptime/backup/restore obligations, cost management, rate limiting.
- Legal surface: ToS, privacy policy, "not financial advice" disclaimers;
  keep the public edition **paper-trading-only** to stay far from
  brokerage/advisory regulatory territory. (Not legal advice; consult one
  before charging money or touching live orders for third parties.)
- PM reality check: do not build multi-tenancy before real users ask.
  Validate with the family edition and a handful of downloadable-edition
  users first.

# 5. Suggested milestone frame (future, in dependency order)

```text
M-CLOUD1   Single-tenant cloud deployment of own instance
           (KMS secrets, Cloud SQL, CI/CD, Terraform, monitoring)
M-DIST1    Downloadable edition (SQLite adapter, single binary,
           auto-update, signing)  [after EZ-01/02]
M-DT1      Playbook pre-commitment + session review (day trading part 1)
M-DT2      Intraday streaming data layer (day trading part 2)
           [after M-PT]
M-SAAS1    AuthN/Z + operator identity + tenant scoping (design ADR first)
M-SAAS2    Multi-tenant hosted beta (paper-only)
```

Each would need its own detailed implementation plan note (same pattern as
[[20260709-paper-trading-implementation-plan]]) before any coding-agent
work. M-CLOUD1 and M-DIST1 are independent of each other; both depend on
the current EZ work. M-DT2 and M-SAAS* are the two largest items.

# 6. Bottom-line recommendations

1. One codebase, editions by configuration — never fork.
2. Day trading = playbook pre-commitment + session review + intraday data,
   sequenced after paper trading; the persona mechanism itself is the easy
   part, and the discipline angle is the differentiator.
3. Web-based: yes, eventually — but climb the staircase: own-instance
   cloud deployment first (max career value, minimal risk), downloadable
   edition second, multi-tenant SaaS only on demonstrated demand.
4. One cloud deep + Terraform beats three clouds shallow for job-market
   signaling; the live product + architecture writeup is the portfolio.
5. Public-facing editions stay paper-trading-only until there is a serious
   reason (and legal review) to change that.
