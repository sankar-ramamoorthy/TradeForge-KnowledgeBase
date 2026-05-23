---
title: TF-F056 Synthesis - Missing Default Advisory Provider Bootstrap
type: processed-synthesis
status: processed
tags: [TradeForge, TF-F, feedback, bug, docker, advisory-provider]
created: 2026-05-23
updated: 2026-05-23
source_history:
  - knowledge/raw/feedback docker compose up bug.md
  - knowledge/raw/20260523-tf-f056-diagnosis.md
  - knowledge/raw/20260523-tf-f056-planning.md
related:
  - "[[AI Advisory Boundary]]"
  - "[[Human Decision Sovereignty]]"
  - "[[Architectural Simplicity]]"
issues:
  - TF-F056
---

# TF-F056 Synthesis - Missing Default Advisory Provider Bootstrap

## Feedback Identified

Docker startup failed during `src.app.api.application` import because
`create_app()` called `_default_advisory_provider(_credential_store)` but the
helper function was missing.

## Root Cause

The application composition root retained the call site for default AI advisory
provider construction, but the function that should decrypt the LiteLLM
credential and construct `OpenAICompatibleAdvisoryProvider` was absent. This
appears consistent with a merge conflict resolution that removed the helper.

An adjacent import gap also existed: `ProviderBootstrapService.reload()` handled
`MasterKeyNotConfiguredError`, but the symbol was not imported in
`application.py`.

## Change Made

Restored `_default_advisory_provider()` in `src/app/api/application.py`.

The helper:

- returns `None` when no credential store exists
- returns `None` when no `litellm` credential is configured
- returns `None` when the configured credential cannot be decrypted or validated
- constructs `OpenAICompatibleAdvisoryProvider` only from a valid decrypted
  LiteLLM credential

Added focused regression tests covering:

- app startup with no LiteLLM credential
- `/advisory/health` returning `not_configured` in that degraded state
- app startup with a valid LiteLLM credential building the concrete provider
- app startup with an unreadable LiteLLM credential still succeeding

## Invariants Involved

- AI Advisory Boundary: provider construction remains advisory-only and does not
  create lifecycle authority.
- Human Decision Sovereignty: advisory availability does not affect lifecycle,
  replay, workspace, market, or admin startup.
- Architectural Simplicity: the fix restores a missing composition helper rather
  than adding a new provider architecture.

## Replay Or Lifecycle Implications

No event, replay, lifecycle, credential schema, frontend, or Docker Compose
behavior changed. The fix only restores safe app composition.

## Closure Against Original Feedback

The original `NameError` path was verified with:

```text
docker compose run --rm tradeforge uv run python -c "from src.app.api.application import app; print(app.title)"
```

The command completed and printed:

```text
TradeForge Runtime
```

This confirms the Docker import/startup path no longer fails on the missing
`_default_advisory_provider` symbol.
