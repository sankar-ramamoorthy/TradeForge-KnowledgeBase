---
title: TF-0066 Implementation - Replay Summarization Assistance
type: raw-implementation-capture
status: raw
created: 2026-05-19
issue: TF-0066
milestone: M11
---

# TF-0066 Implementation - Replay Summarization Assistance

Implemented `ReplayAdvisoryService` as a services-layer bridge from `ReplayTimeline` into the TF-0065 advisory contract.

The service builds source-linked `AdvisoryRequest` values and delegates generation through `AIAdvisoryService`.

No event-store dependency, lifecycle dependency, persistence, API endpoint, frontend surface, or concrete LLM adapter was introduced.

Verification:

- `uv run pytest tests\test_replay_advisory_service.py tests\test_ai_advisory_interfaces.py`
- `uv run ruff check src\domain\advisory src\services\advisory tests\test_ai_advisory_interfaces.py tests\test_replay_advisory_service.py`
- `uv run mypy src\domain\advisory src\services\advisory tests\test_ai_advisory_interfaces.py tests\test_replay_advisory_service.py`

