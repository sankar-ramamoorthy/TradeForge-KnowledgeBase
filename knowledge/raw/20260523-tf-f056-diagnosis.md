---
title: TF-F056 Diagnosis - Missing Default Advisory Provider Bootstrap
type: diagnosis
status: raw
tags: [tradeforge, feedback, bug, tf-f056, docker, advisory-provider]
created: 2026-05-23
source:
  - knowledge/raw/feedback docker compose up bug.md
---

# TF-F056 Diagnosis - Missing Default Advisory Provider Bootstrap

## Source Feedback

`knowledge/raw/feedback docker compose up bug.md` reports a Docker startup crash:

```text
File "/app/src/app/api/application.py", line 447, in create_app
  else _default_advisory_provider(_credential_store)
       ^^^^^^^^^^^^^^^^^^^^^^^^^^
NameError: name '_default_advisory_provider' is not defined.
```

The operator also suspected a merge conflict may have deleted the missing code.

## Observable Gap

`docker compose up` starts the runtime container, imports
`src.app.api.application`, reaches module-level `app = create_app()`, and exits
because `create_app()` references `_default_advisory_provider()` but no such
function exists.

## Root Cause

The app composition root still contains the call site for default advisory
provider construction:

```python
_resolved_advisory_provider = (
    ai_advisory_provider
    if ai_advisory_provider is not None
    else _default_advisory_provider(_credential_store)
)
```

The factory function that should read the LiteLLM credential and construct
`OpenAICompatibleAdvisoryProvider` is absent. The imports for
`OpenAICompatibleAdvisoryProvider`, `get_litellm_credential`, and
`LiteLLMCredentialNotConfiguredError` are present, which supports the merge-loss
hypothesis.

There is a second adjacent risk: `ProviderBootstrapService.reload()` catches
`MasterKeyNotConfiguredError`, but `application.py` no longer imports that name.
That path may crash if reload hits a missing master key.

## Relevant Invariants

- AI Advisory Boundary: startup must not grant AI authority; it should only wire
  a non-authoritative provider when credentials exist.
- Human Decision Sovereignty: advisory provider availability must not affect
  lifecycle authority or execution.
- Architectural Simplicity: the missing code is a composition-root helper, not a
  new architecture.

## Minimum Correct Scope

- Restore `_default_advisory_provider()` in `src/app/api/application.py`.
- Return `None` when LiteLLM is not configured or cannot be safely decrypted, so
  advisory endpoints continue to report unavailable/not-configured rather than
  preventing runtime startup.
- Import the missing credential error used by `ProviderBootstrapService.reload()`.
- Add regression tests proving `create_app()` starts without LiteLLM and builds
  an `OpenAICompatibleAdvisoryProvider` when LiteLLM credentials are configured.

## Explicitly Out Of Scope

- Changing advisory prompt behavior.
- Changing the LiteLLM credential schema.
- Adding new frontend surfaces.
- Changing Docker Compose configuration.
- Promoting or processing unrelated raw notes.
