---
title: LiteLLM Compose Runtime Gap
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, litellm, docker-compose, ai-advisory, operational-gap]
created: 2026-05-23
---

# LiteLLM Compose Runtime Gap

## Trigger

Operator observation on 2026-05-23 while preparing the TradeForge advisory stack for local use.

## Raw Input

> TradeForge expects litellm to be on localhost:4000 i think. lets create a docker container to host litellm.

## Observations

- TradeForge has an `OpenAICompatibleAdvisoryProvider` and a `litellm` credential shape.
- Runtime documentation and tests use `http://localhost:4000` as the local LiteLLM endpoint.
- `docker-compose.yml` currently starts TradeForge and Postgres, but not LiteLLM.
- This leaves the AI advisory path operationally incomplete for a Docker-based local setup.
- LiteLLM remains advisory infrastructure only; it must not gain lifecycle or event authority.

## Ideas

- Add a LiteLLM service to Docker Compose.
- Expose LiteLLM on host port `4000` so a host-run backend can use `http://localhost:4000`.
- Allow Compose-network access through `http://litellm:4000` when the TradeForge backend also runs in Docker.
- Keep provider API keys out of Git by routing model credentials through environment variables.
- Track the work as a feedback-originated operational issue.

## Questions

- Should the LiteLLM service start by default or only under an optional Compose profile?
- Which default model alias should TradeForge operators register in the `litellm` credential?
- Should the documentation prefer `localhost:4000` for host-run backend and `litellm:4000` for container-run backend?

## Concerns

- Requiring LiteLLM environment variables during ordinary `docker compose up` could break the non-AI runtime path.
- Hardcoding provider API keys or model-specific secrets would violate the credential boundary.
- Putting LiteLLM in Compose must not imply that AI advisory is mandatory for lifecycle, replay, or market workflows.
- `localhost` means different things from the host versus inside the TradeForge container.

## Possible Next Outputs

- Issue candidate: add optional LiteLLM Docker Compose service and setup documentation.
- Diagnosis note.
- Planning note.
- Runtime docs update.
