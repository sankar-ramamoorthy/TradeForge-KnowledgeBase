• Proposed Plan


  # M14 Milestone Plan: Behavioral Intelligence And Cognitive Auditability

  ## Summary

  M14 should start by formalizing the existing roadmap issues TF-C001 through TF-C010 in DOCS/ISSUE_REGISTER.md, then implement a thin deterministic
  behavioral-signal foundation before any clustering, AI-assisted interpretation, or decision-quality scoring.

  The first vertical slice is TF-C001: Detect recurring sizing violations. It is deterministic, replayable, grounded in existing decision.plan_created,
  execution, and review.review_completed events, and establishes the behavioral signal pattern for the rest of M14.

  M14 must treat behavioral intelligence as derived/review intelligence unless a deterministic review evaluation event is explicitly recorded. It must not
  create lifecycle authority, execution authority, AI authority, or hidden state.

  ## Key Changes

  - Add detailed issue-register entries for:
      - TF-C001: Detect recurring sizing violations
      - TF-C002: Detect impulsive execution patterns
      - TF-C003: Implement process deviation overlays
      - TF-C004: Implement behavioral clustering
      - TF-C005: Implement recurring mistake analysis
      - TF-C006: Implement discipline deterioration signals
      - TF-C007: Implement thesis attachment analysis
      - TF-C008: Implement emotional reflection overlays
      - TF-C009: Implement operator behavior timelines
      - TF-C010: Implement decision-quality review metrics
  - Add a KB raw planning capture for the M14 plan under knowledge/raw/YYYYMMDD-m14-behavioral-intelligence-planning.md, then process it through the KB
    loop if durable semantic updates are needed.
  - Add an ADR checkpoint before implementation. A new ADR is likely warranted if M14 introduces a durable behavioral signal model, new review event
    semantics, or cross-decision behavioral projections.
  - Keep behavioral outputs separated by authority:
      - Canonical: lifecycle, execution, and review events already in the ledger.
      - Derived: deterministic behavioral signals and overlays rebuilt from events.
      - Advisory: clustering, narrative summaries, emotional interpretation, and AI-assisted review.

  ## Implementation Sequence

  1. TF-C001: Detect recurring sizing violations
      - Affected layer: domain, services, app, tests.
      - Build deterministic BehavioralSignal / SizingViolationSignal domain objects as derived outputs, not canonical state.
      - Input events: decision.plan_created, decision.plan_revised where available, execution/position events, and review.review_completed.
      - Use existing structured plan sizing_rationale and review discipline_observations / execution_quality as replayable inputs.
      - Expose a read API that returns derived sizing signals scoped by persona, workspace, and optionally decision.
      - Acceptance: repeated sizing-risk patterns can be detected from event history, results are deterministic, legacy events degrade gracefully, and no
        lifecycle transition is created.
  2. TF-C002: Detect impulsive execution patterns
      - Affected layer: domain, services, app, tests.
      - Use lifecycle timestamps and stage ordering to detect execution shortly after approval/arming, execution without meaningful plan context, or
        execution inconsistent with trigger assumptions.
      - Keep outputs derived and explainable with source event references.
      - Acceptance: impulsive-pattern signals are reproducible from event history and distinguish timing/process concerns from trade outcomes.
  3. TF-C003: Implement process deviation overlays
      - Affected layer: services, app, frontend, tests.
      - Surface TF-C001 and TF-C002 outputs in replay/review/workspace surfaces as behavioral overlays.
      - Do not create dashboard-style analytics; overlays should support review and cognitive auditability.
      - Acceptance: review and replay can show why a behavioral signal appeared, which source events contributed, and that the signal is derived.
  4. Higher-order M14 expansion
      - TF-C004 clustering only after deterministic signals exist.
      - TF-C005 recurring mistake analysis should aggregate deterministic signals plus review reflections.
      - TF-C006 discipline deterioration should use longitudinal signal frequency/severity, not outcome P&L.
      - TF-C007 thesis attachment should compare thesis revisions, invalidation conditions, review reflections, and delayed exits where events support it.
      - TF-C008 emotional overlays should begin from operator-authored review text, not inferred emotion as fact.
      - TF-C009 behavior timelines should compose deterministic signals into replayable chronological projections.
      - TF-C010 decision-quality metrics should remain reflective and review-scoped, not a trading score or approval gate.

  ## Public Interfaces And Types

  - Add derived behavioral signal contracts with fields equivalent to:
      - signal_id
      - signal_type
      - persona_id
      - workspace_id
      - decision_id
      - severity
      - summary
      - source_event_refs
  - Add read endpoints only, likely under a behavioral/review namespace such as:
      - GET /behavioral/signals
      - GET /behavioral/signals/{decision_id}
  - Do not add mutating behavioral endpoints in the initial slice.
  - Do not add new event types for TF-C001 unless ADR review concludes deterministic review evaluations should be captured as review.* facts.

  ## Test Plan

  - Domain tests for deterministic signal detection, source event references, severity assignment, and legacy empty-payload handling.
  - Service tests proving signals are rebuilt from event history and remain stable across event ordering by timestamp/source sequence.
  - API tests for persona/workspace/decision filtering and derived-authority response shape.
  - Replay tests proving behavioral overlays can be reconstructed without live APIs, current AI output, UI state, or mutable projections.
  - Frontend tests for TF-C003 ensuring overlays render as derived behavioral context, not canonical truth.

  ## Assumptions


─ Worked for 4m 36s ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────


› Implement the plan.


• I’ll move from the plan into a bounded M14/TF-C001 implementation. First I’m checking the current repo state and the existing event/API composition points
  so I can add the issue records, then the smallest deterministic behavioral signal slice.