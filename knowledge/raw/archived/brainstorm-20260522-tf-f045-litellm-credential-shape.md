---
title: TF-F045 LiteLLM Credential Shape Implementation
type: brainstorm
status: raw
tags: [tradeforge, m13, tf-f045, litellm, credential-boundary, ai-advisory]
created: 2026-05-22
---

# TF-F045 LiteLLM Credential Shape Implementation

## Trigger

M13 advisory adapter work needs a configured LLM endpoint without putting LiteLLM
URLs, API keys, or model strings in application code or environment files.
TF-F045 was selected as the first M13 LLM adapter prerequisite.

## Raw Input

User asked Codex to use the runtime development loop and start the next M13
issue, TF-F045, without promoting anything from the raw folder. After initial
implementation paused for API key consideration, the user confirmed that if
TF-F045 was already done, Codex should finish the loop, create KB raw notes, and
commit.

## Observations

- The existing credential boundary already stores encrypted provider payloads in
  `.keys.enc` using `CredentialStore` and `KeyManager`.
- ADR-0037 requires all future provider credentials, including LLM providers, to
  flow through `CredentialStore` rather than raw environment variables.
- `CredentialStore` is intentionally generic, so TF-F045 did not require changing
  its persistence format.
- The missing piece was a typed LiteLLM payload shape and a CLI path for setting
  or rotating that payload using the existing encrypted workflow.

## Ideas

- Add a small `src/security/litellm_credential.py` helper rather than making
  `CredentialStore` provider-aware.
- Keep `CredentialStore` generic and let `create_litellm_credential()` own the
  provider-specific field shape: `base_url`, `api_key`, and `default_model`.
- Add `get_litellm_credential()` so later TF-F046 adapter work has a clear
  retrieval boundary and a typed `LiteLLMCredentialNotConfiguredError`.
- Extend `scripts/manage_credentials.py register litellm` with `--base-url`,
  `--api-key`, and `--default-model`, preserving the existing registration and
  replacement behavior.

## Questions

- Whether future advisory tasks will need per-task model selection in addition
  to the current `default_model` credential field.
- Whether LiteLLM health checks should decrypt the credential directly or call
  through the future adapter boundary.
- Whether the CLI should eventually get a provider-specific subcommand for a
  cleaner LiteLLM setup experience.

## Concerns

- The LiteLLM base URL and model examples used in tests are placeholder values,
  not operational configuration.
- No application composition wiring was added; TF-F046 remains responsible for
  consuming this credential shape.
- This implementation must not imply advisory authority, lifecycle authority, or
  canonical event truth. Credentials remain operational capability metadata.

## Possible Next Outputs

- Process this raw note after TF-F046 begins or after the LLM adapter path is
  validated end to end.
- Use the note as background for TF-F046 and TF-F052, especially around
  not-configured error handling.
- No canonical promotion until the API key and LiteLLM operating workflow are
  validated.
