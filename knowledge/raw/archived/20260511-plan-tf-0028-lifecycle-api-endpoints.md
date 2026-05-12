 Design Plan

  Issue discipline:

  - Issue ID: TF-0028
  - Milestone: M7
  - Title: Add lifecycle API endpoints
  - Affected layer: app, services
  - Impacted ADRs: ADR 0002, ADR 0020
  - Impacted invariants: Decision Lifecycle, Human Decision Sovereignty, plus Event Sourcing and Layer Separation as supporting constraints
  - Acceptance criteria in scope:
      - endpoints validate through lifecycle orchestration
      - invalid transitions return explicit errors
      - accepted transitions append events through the event store port

  KB/runtime alignment:
  - Events must remain fact-backed, immutable, and append-only.
  - FastAPI is an app-layer adapter only; it can parse requests and translate service outcomes, but not own lifecycle logic or persistence semantics.

  Implementation shape:

  POST /lifecycle/transitions
      -> request DTO parsed in app layer
      -> app dependency resolves LifecycleOrchestrationService
      -> service derives current lifecycle state from EventStore.read_events()
      -> service validates requested next stage
      -> if valid: append canonical lifecycle event via EventStore.append()
      -> route returns explicit result payload
      -> if invalid: route returns 409 with validation details

  Module plan:

  - src/app/api/routes.py
      - add request/response Pydantic models for lifecycle transitions
      - add route handler and explicit HTTP error mapping
      - keep health route unchanged
  - src/app/api/application.py
      - allow app factory injection of LifecycleOrchestrationService
      - default to an app-owned in-memory event store for local/runtime isolation unless a service is injected
  - tests/test_fastapi_runtime.py
      - replace the old “404 for lifecycle” expectation with endpoint behavior tests
      - cover valid first transition, invalid skipped transition, and event persistence across requests within one app instance
  - DOCS/ISSUE_REGISTER.md
      - mark TF-0028 complete and record verification if implementation and tests pass

  Boundary decisions:

  - No direct Postgres access from routes.
  - No lifecycle rule duplication in HTTP models or handlers.
  - No new hidden state beyond the app’s injected event-store-backed service instance.
  - No scope expansion into replay or workspace APIs.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• I’ve got the design bounded. I’m editing the FastAPI app and tests now so the new endpoint is only an adapter over the existing lifecycle service, with
  explicit conflict errors for invalid transitions and no lifecycle logic in the route layer.
