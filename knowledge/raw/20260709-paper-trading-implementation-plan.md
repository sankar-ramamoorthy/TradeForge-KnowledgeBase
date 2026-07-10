---
title: Paper Trading Implementation Plan — Alpaca Paper Execution Boundary
type: implementation-plan
status: raw
tags: [tradeforge, paper-trading, execution, alpaca, broker-boundary, implementation-plan]
created: 2026-07-09
updated: 2026-07-09
---

# Paper Trading Implementation Plan — Alpaca Paper Execution Boundary

Proposed milestone: **M-PT — Paper Execution And Outcome Truth**.
Derived from [[20260709-comprehensive-product-audit]] (finding F2: the
lifecycle loop never closes without manual data entry).

This plan is written for execution by Codex/Sonnet-class coding agents in
small, independently verifiable issues. Each issue must follow runtime
`AGENTS.md` discipline: register in `DOCS/ISSUE_REGISTER.md` first, confirm
affected layers/ADRs/invariants, plan, then implement.

---

# 1. Why Paper Trading, Why Now

- Closes `Plan → Approval → Execution → Position → Review` with real fills,
  real prices, real slippage — zero financial risk.
- Makes [[ReviewReflection]] honest: thesis-vs-outcome uses broker facts,
  not hand-typed numbers.
- The Armed state finally means something: the system can watch trigger
  conditions instead of the operator doing it mentally.
- `alpaca-py` is already a dependency and Alpaca credentials
  (`api_key` + `secret_key`) already have a registered shape in the
  credential store. The paper API is the same SDK with `paper=True`.

# 2. Invariant Compliance (non-negotiable design frame)

- **Human sovereignty**: only an explicit human command submits, replaces,
  or cancels an order. No advisory pathway may reach the execution port.
  AI must have no code path to order submission.
- **Event sourcing**: broker interactions are recorded as immutable facts
  (submitted, accepted, filled, cancelled, rejected). The broker is not a
  state store TradeForge trusts — the [[EventLedger]] remains canonical;
  broker state is reconciled *into* events, never read live during replay.
- **Lifecycle integrity**: paper execution slots into the existing
  Execution/Position stages; no stage is skipped. An order can only be
  submitted from an Approved or Armed plan.
- **Layer separation**: broker SDK code lives only in `infrastructure/`;
  domain defines pure order/fill types and validation; services orchestrate;
  app exposes routes.
- **Paper-only guard**: a hard, multi-layer guard (see TF-P003) makes live
  trading impossible in this milestone, even with live keys configured.

# 3. Target Architecture

```text
Operator clicks "Submit Paper Order" (Plan Review / Active Position)
→ app route (POST, explicit human command)
→ ExecutionOrchestrationService (validates lifecycle stage + plan fields)
→ ExecutionPort (domain-facing protocol)
→ AlpacaPaperExecutionAdapter (infrastructure, alpaca-py, paper=True)
→ broker ack
→ PaperOrderSubmitted event appended (fact, with provenance)

OrderSyncService (polling, background)
→ reads open paper orders from ledger projection
→ queries Alpaca paper API for status changes
→ appends PaperOrderAccepted / PaperOrderFilled / PaperOrderCancelled /
  PaperOrderRejected events (facts observed, timestamped, provider-tagged)
→ position projection updates; Armed triggers evaluated
```

New domain event types (append-only, facts only):

```text
PaperOrderSubmitted        (order intent + broker order id + plan linkage)
PaperOrderAccepted
PaperOrderPartiallyFilled  (qty, price, timestamp from broker)
PaperOrderFilled           (avg price, qty, broker timestamps)
PaperOrderCancelled
PaperOrderRejected         (reason string from broker)
PaperPositionClosed        (realized P&L as reported facts)
```

Naming keeps the `Paper` prefix so a future live boundary is a separate,
deliberate event family — no silent reuse.

# 4. Issue Breakdown

## Phase 0 — Doctrine And Contracts

### TF-P001: ADR — Paper Execution Boundary Model

- Write ADR (next free number) covering: ExecutionPort contract; paper-only
  scope; event taxonomy above; broker-as-reconciled-external-system (mirrors
  ADR-0032 provider boundary); human-command-only invocation; replay safety
  (sync events are historical facts; replay never calls the broker).
- Update `EVENT_TAXONOMY.md` (KB) and glossary entries: [[ExecutionIntent]],
  new [[PaperOrder]], [[ExecutionPort]].
- Acceptance: ADR accepted; invariants doc unchanged (no invariant needs
  weakening — if the design seems to require it, the design is wrong).

### TF-P002: Register M-PT issues + roadmap entry

- Add TF-P001…TF-P012 to `DOCS/ISSUE_REGISTER.md` with milestone M-PT,
  layers, ADR links, acceptance criteria. Add M-PT to the active roadmap.

## Phase 1 — Domain

### TF-P003: Paper order domain model and validation

- `src/domain/execution/` (new package): `PaperOrderIntent` (symbol, side,
  qty, order type limit/market, limit price, time-in-force, plan_id,
  decision_id), order state machine (submitted → accepted →
  partially_filled/filled | cancelled | rejected), pure validators:
  - order must reference an existing plan in Approved or Armed stage
  - qty > 0; limit orders require limit price
  - `mode` field hard-fixed to literal `"paper"`; constructing a non-paper
    intent raises. This is guard layer 1 of 3.
- New event dataclasses per taxonomy above, following existing event
  patterns in `src/domain/events/`.
- Tests: state machine transitions, validation rejections, event immutability.
- No I/O, no SDK imports (domain purity).

## Phase 2 — Infrastructure

### TF-P004: ExecutionPort protocol + AlpacaPaperExecutionAdapter

- Port protocol in domain or a ports module consistent with
  [[EventStorePort]] precedent: `submit_order`, `cancel_order`,
  `get_order_status`, `get_open_orders`, `get_position`.
- Adapter in `src/infrastructure/execution/alpaca_paper.py` using
  `alpaca.trading.client.TradingClient(api_key, secret_key, paper=True)`.
  `paper=True` is hardcoded, not configurable — guard layer 2.
- Credential resolution through existing CredentialStore alpaca shape at the
  composition root (same wiring pattern as market adapters, TF-F006).
- Map Alpaca responses to domain types; broker timestamps and raw order ids
  preserved for provenance.
- Tests: adapter with mocked SDK client (follow `test_alpaca_adapter.py`
  conventions); assert TradingClient constructed with `paper=True`;
  error mapping (auth failure, rejected order, network error → typed errors).

### TF-P005: In-memory FakeExecutionAdapter

- Deterministic fake implementing ExecutionPort with scriptable fills —
  default adapter when no Alpaca credential exists, and the test backbone.
  Instant-fill and never-fill modes. Lets the whole feature work in the
  zero-key quickstart (family mode) with simulated fills at snapshot prices.

## Phase 3 — Services

### TF-P006: ExecutionOrchestrationService

- `src/services/execution/orchestration.py`: the only caller of
  ExecutionPort. Public methods take an explicit operator command object;
  no advisory module may import this service — add an import-boundary test
  asserting `src/services/advisory/` has no dependency path to it
  (guard layer 3).
- `submit_paper_order(command)`: validate lifecycle stage via existing
  lifecycle read models → call port → append `PaperOrderSubmitted`.
- `cancel_paper_order(command)` symmetrical.
- On submit, if decision is in Approval/Armed, drive the existing
  transition to Execution stage through [[LifecycleOrchestrationService]]
  (reuse, don't duplicate, transition rules).
- Tests: stage rejection (cannot submit from Thesis), event append
  verification, no-write-on-broker-failure semantics (broker error → no
  submitted event, error surfaced).

### TF-P007: OrderSyncService (polling reconciliation)

- Background asyncio task in the FastAPI lifespan (matches existing runtime
  patterns; no new infra): every N seconds (default 30, env-configurable)
  fetch status for ledger-open orders; append status-change events
  idempotently (dedupe on broker order id + status + fill qty).
- Never deletes/mutates; only appends newly observed facts.
- Graceful degradation: broker unreachable → log + retry, no crash, no
  fabricated events.
- Tests: idempotent re-poll, partial-fill sequence, out-of-order responses.

### TF-P008: Armed-trigger evaluation (optional within M-PT, high value)

- Extend sync loop: for Armed plans with structured trigger conditions
  (price crossing), evaluate against latest persisted snapshot; when met,
  raise an *attention item* ("trigger condition met — review and submit?")
  in the existing [[OperationalAttentionQueue]]. Deliberately does NOT
  auto-submit — sovereignty preserved. Auto-submit-on-trigger is a separate
  future ADR discussion.

## Phase 4 — API + Frontend

### TF-P009: Execution API routes

- New router `execution_router` prefix `/execution` in a NEW file
  `src/app/api/execution_routes.py` (do not grow routes.py — see audit F6):
  - `POST /execution/paper-orders` (submit; body = command)
  - `POST /execution/paper-orders/{order_id}/cancel`
  - `GET  /execution/paper-orders?decision_id=`
  - `GET  /execution/paper-positions?decision_id=`
- FastAPI tests per existing `test_fastapi_runtime.py` conventions.

### TF-P010: Workspace surfaces

- Plan Review workspace: "Submit Paper Order" panel visible only in
  Approved/Armed stages; prefilled from [[StructuredTradePlan]] fields
  (entry, size); explicit confirm dialog restating symbol/side/qty/price.
- Active Position workspace: live order status panel (submitted / accepted /
  filled with avg price), position facts, cancel button.
- A persistent **PAPER** badge on every execution surface — visual guard.
- Review workspace: actual-fill facts (entry/exit price, slippage vs plan)
  displayed beside the operator's manual reflection inputs.
- Follow existing modal/panel patterns; typecheck + build gates.

## Phase 5 — Review Integration And Closeout

### TF-P011: Execution-quality facts in review projections

- Derive deterministic read-model fields: planned vs actual entry, slippage,
  holding period, realized P&L (from broker facts). Feed the existing
  behavioral signals layer (sizing deviation now computable from real fills).
  Authority-labeled `derived`, per M14 conventions.

### TF-P012: Docs, demo, KB synthesis

- Update README (paper trading quickstart), HOW-TO-SETUP-KEYS (paper keys
  note: same alpaca credential works), demo flow option using
  FakeExecutionAdapter; KB processed synthesis after completion.

# 5. Testing Strategy (cross-cutting)

- Every service test runs against FakeExecutionAdapter; adapter tests mock
  the SDK; one optional manually-run integration test against a real Alpaca
  paper account (skipped in CI, gated by env var).
- Replay regression: replaying a decision containing paper events must make
  zero broker calls (assert via fake with call counter).
- Guard tests: (1) domain rejects non-paper mode, (2) adapter pins
  `paper=True`, (3) no import path from advisory to execution service.

# 6. Suggested Execution Order For Coding Agents

```text
TF-P001 → TF-P002 → TF-P003 → TF-P005 → TF-P004 → TF-P006 → TF-P009(min)
→ TF-P010(min: submit+status) → TF-P007 → TF-P010(rest) → TF-P011
→ TF-P008 → TF-P012
```

Each step lands green (pytest + ruff + mypy + frontend typecheck/build)
before the next. Fake-first ordering (P005 before P004) lets everything be
demoable with zero keys from the earliest possible point.

# 7. Open Questions For The Operator

- Fractional shares in paper mode? (Alpaca supports; suggest yes, qty as
  Decimal from day one.)
- Should Armed auto-submit ever be allowed? (This plan says no; revisit
  with a dedicated ADR only after months of paper usage.)
- One shared Alpaca paper account for the household, or per-operator
  accounts once RAMP-03 (operator identity) lands? Suggest: shared first.
