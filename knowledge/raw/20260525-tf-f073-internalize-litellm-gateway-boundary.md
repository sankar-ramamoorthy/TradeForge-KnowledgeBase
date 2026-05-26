---
title: TF-F073 Internalize LiteLLM Gateway Network Boundary Implementation Note
type: implementation-note
status: raw
created: 2026-05-25
issue: TF-F073
milestone: M13B
tags:
  - tf-f073
  - m13b
  - litellm
  - docker
  - advisory-runtime
---

# TF-F073 Internalize LiteLLM Gateway Network Boundary Implementation Note

## Context

TF-F073 internalizes the LiteLLM runtime boundary after TF-F072 established
TradeForge-mediated advisory model selection.

The goal is to make TradeForge the operator-facing advisory gateway while
LiteLLM remains managed infrastructure on Docker internal networking by
default.

## Implementation Summary

The default `docker-compose.yml` no longer publishes LiteLLM on host port
`4000`. The service now uses:

```yaml
expose:
  - "4000"
```

This keeps LiteLLM reachable to other Compose services on the Docker network
while avoiding direct browser/host access by default.

An explicit debug override was added:

```text
docker-compose.litellm-debug.yml
```

The override publishes `4000:4000` only when the operator intentionally starts
Compose with that file.

Docs now direct the managed runtime credential to `http://litellm:4000` and
describe the debug override as the temporary local inspection path.

## Boundary Notes

No advisory API behavior changed.

No event types were added.

No lifecycle behavior changed.

No downstream LLM provider secret governance was added; TF-F074 owns that.

Provider Governance and smoke tests continue to operate through TradeForge.

## Verification

- `uv run pytest tests/test_postgres_compose.py tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py`
- `docker compose config`
- `docker compose --profile advisory config`
- `docker compose --profile advisory -f docker-compose.yml -f docker-compose.litellm-debug.yml config`
