---
title: TF-F057 Diagnosis - LiteLLM Compose Runtime Service
type: diagnosis
status: raw
tags: [tradeforge, tf-f057, diagnosis, litellm, docker-compose, ai-advisory]
created: 2026-05-23
issues: [TF-F057]
source:
  - knowledge/raw/brainstorm-20260523-litellm-compose-runtime.md
---

# TF-F057 Diagnosis - LiteLLM Compose Runtime Service

## Observable Gap

TradeForge advisory generation expects an OpenAI-compatible LiteLLM endpoint, and local examples use `http://localhost:4000`. The Docker Compose runtime does not provide a LiteLLM service, so an operator can start TradeForge and Postgres but still lacks the local LLM proxy required for advisory tasks.

## Root Cause

The application-side advisory boundary is implemented, but the local runtime stack has not been extended to include the external LiteLLM proxy process. This is an operational composition gap, not a domain, lifecycle, event, or advisory contract gap.

There is also a host/container addressing distinction:

- Host-run backend should use `http://localhost:4000`.
- Container-run backend should use `http://litellm:4000` on the Compose network.

## Relevant Invariants

- AI Advisory Boundary: LiteLLM only supplies non-canonical advisory outputs.
- Human Decision Sovereignty: LiteLLM must not approve, execute, or mutate lifecycle state.
- Derived State Must Remain Distinguishable: generated advisory responses remain non-canonical.
- Architectural Simplicity: use Compose wiring and existing credential boundaries instead of new runtime abstractions.

## Minimum Correct Scope

- Add an optional LiteLLM Compose service.
- Add a checked-in, secret-free LiteLLM config that reads provider keys from environment variables.
- Expose host port `4000`.
- Document base URL selection for host-run and container-run backends.
- Add focused tests for Compose/config presence.

## Out Of Scope

- Changing advisory domain contracts.
- Changing event schemas or lifecycle states.
- Moving provider secrets into Git, `.env`, or application code.
- Making AI advisory required for normal runtime startup.
- Implementing automatic advisory enrichment.

## ADR Checkpoint

No new ADR is required. The change is operational infrastructure within existing ADRs:

- ADR-0011 governs local Docker Compose runtime tooling.
- ADR-0037 governs credential boundaries and forbids Git-stored provider secrets.
- ADR-0041 and ADR-0042 govern advisory/cognitive evidence boundaries.
