 Here's what was written:

  ---
  ISSUE_REGISTER.md — 8 edits:

  1. Index table — 10 new rows added (TF-F045–TF-F054), all Planned, all milestone M13.
  2. TF-B001 — Implementation context added: entire domain model, stores, capture service, and draft service were built in M12. This issue is verification +
   test coverage, not ground-up implementation.
  3. TF-B002 — Implementation context added: ContextualWeight enum and store filtering exist from M12. The new work is the weight-assignment service logic.
  4. TF-B005 — Implementation context added: AdvisoryConfidenceRange enum and storage exist from M12. New work is UX surface.
  5. TF-B006 — Implementation context added: ThesisInfluenceSummary already exists in src/services/advisory/interpretation.py. New work is influence history
   over time and workspace wiring.
  6. TF-B007 — Implementation context added: ThesisInfluence enum values and query filtering exist from M12. New work is the operator-facing split surface.
  7. TF-B014 — Dependency note added: blocked on TF-F046. InterpretationDraftService is implemented and waiting for a concrete provider.
  8. TF-F045–TF-F054 full specs appended — each with problem, scope, affected layer, ADR links, invariants, and acceptance criteria.

  Milestone_Roadmap_v2.md — 1 edit:
  M13 linked issues section restructured into three groups: LLM adapter prerequisites, interpretation domain/infrastructure, and replay/UX surfaces — with
  notes on what M12 already built and the TF-F046 → TF-B014 dependency called out explicitly.

  The project is ready to start M13. The correct first issue to implement is TF-F045.