---
title: TF-0004 And TF-0005 Runtime Scaffold Plan
type: implementation-plan
status: processed
tags: [TradeForge, runtime, scaffold, Docker, pytest, ADR-0011]
created: 2026-05-08
updated: 2026-05-08
source_notes:
  - knowledge/raw/proposed TF-0004 And TF-0005 Runtime Scaffold Plan.md
---

# TF-0004 And TF-0005 Runtime Scaffold Plan

## Status

Processed runtime planning note. This is not canonical semantic architecture and does not mark the linked runtime issues as complete.

## Summary

The next two M1 runtime scaffold issues should be implemented separately:

- TF-0004: Add `docker-compose.yml` for local development.
- TF-0005: Add pytest baseline and repeatable test command.

Both are infrastructure/app scaffold work linked to [ADR 0011: Runtime Development Environment](../../../../TradeForge/DOCS/adr/0011-runtime-development-environment.md).

Neither issue should add domain models, event semantics, services, brokers, databases, or runtime APIs.

## TF-0004 Plan

Create root `docker-compose.yml` with one service:

- service name: `tradeforge`
- build context: `.`
- Dockerfile: `Dockerfile`
- image: `tradeforge-runtime:local`
- working directory: `/app`
- bind mount repo source into `/app`
- default command: `uv run python --version`
- no ports, databases, brokers, market data, or extra services

Verification:

```powershell
docker compose config
docker compose build tradeforge
docker compose run --rm tradeforge
```

Expected result:

- Compose config is valid.
- Compose builds from the local Dockerfile.
- Container prints Python 3.12.x.
- Compose remains local development orchestration only and does not imply microservices.

## TF-0005 Plan

Update `pyproject.toml` to add pytest as a dev dependency:

```toml
[dependency-groups]
dev = [
    "pytest>=8.0",
]
```

Add pytest configuration:

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
pythonpath = ["."]
```

Create:

```text
tests/test_scaffold.py
```

Baseline test behavior:

- Confirm the project scaffold can run under pytest.
- Assert Python major/minor is 3.12, matching ADR 0011 and Docker runtime target.

Verification:

```powershell
uv lock
uv sync
uv run pytest
```

## Acceptance Criteria

For TF-0004:

- `docker-compose.yml` exists.
- Compose defines a local TradeForge development service.
- Compose builds from the local Dockerfile.
- Source is available in the container for local iteration.
- Compose does not add databases, brokers, market-data services, or production orchestration.

For TF-0005:

- pytest is available through project dev dependencies.
- A baseline test exists and passes.
- `uv run pytest` is the repeatable test command.
- Test setup does not require live external services.
- No domain behavior tests are introduced yet.

## Assumptions

- Docker commands may require escalation to access Docker Desktop resources.
- `uv lock` may update `uv.lock`, and that lockfile should remain committed for reproducibility.
- README setup documentation remains out of scope until TF-0007.

## KB Boundary

This plan preserves the distinction that runtime scaffold tooling supports implementation but does not define:

- [[Event Ledger]] semantics
- [[Decision Lifecycle Engine]] authority
- [[Persona]] meaning
- [[Workspace]] behavior
- replay rules
- AI advisory boundaries

