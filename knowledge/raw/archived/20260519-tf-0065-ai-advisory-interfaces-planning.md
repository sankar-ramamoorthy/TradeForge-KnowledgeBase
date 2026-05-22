---
title: TF-0065 Planning - AI Advisory Interfaces
type: raw-planning
status: raw
created: 2026-05-19
issue: TF-0065
milestone: M11
---

# TF-0065 Planning - AI Advisory Interfaces

## Issue Scope

TF-0065 defines the runtime AI advisory boundary before any concrete LLM adapter, replay summarizer, review assistant, or advisory provenance store exists.

## Architectural Interpretation

M11 begins with contracts, not generated advice. The runtime needs provider-agnostic advisory request and response models that keep AI output explicitly outside canonical workflow truth.

The boundary must satisfy ADR-0006:

- AI may summarize, rank, suggest interpretations, and help review.
- AI may not write events, approve lifecycle transitions, execute trades, mutate workflow state, or define workspace state.
- AI output must preserve provenance and uncertainty where practical.

## Design Plan

Runtime modules:

- `src/domain/advisory/`
  - pure domain contract models
  - authority classification for advisory artifacts
  - request/response dataclasses
  - provider protocol
- `src/services/advisory/`
  - orchestration-only service for invoking an advisory provider
  - no event store, lifecycle, broker, or persistence dependency
- `DOCS/ai-advisory-interfaces.md`
  - implementation-facing contract for later M11 work

## Event Impact

No new events and no event store writes. Advisory artifacts are non-canonical outputs.

## Replay Impact

Responses include source references and provenance so future persistence/provenance issues can preserve what was generated and from which inputs. TF-0065 does not persist advisory artifacts.

## Lifecycle Impact

No lifecycle transitions or approval paths. Advisory responses must not expose methods that approve plans, execute trades, or append ledger events.

## Testing Strategy

- Domain tests for immutability, validation, authority classification, uncertainty, and source references.
- Service tests for provider invocation and lack of mutation/persistence authority.
- Import-boundary test to prevent advisory domain contracts from importing app, infrastructure, lifecycle services, or event store implementation.

## ADR Checkpoint

No new ADR is required. ADR-0006 is already accepted and directly governs this issue. TF-0065 implements the boundary without changing lifecycle semantics, event taxonomy, persistence architecture, or provider capability architecture.

