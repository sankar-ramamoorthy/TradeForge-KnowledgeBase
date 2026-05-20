---
title: TF-0066 Planning - Replay Summarization Assistance
type: raw-planning
status: raw
created: 2026-05-19
issue: TF-0066
milestone: M11
---

# TF-0066 Planning - Replay Summarization Assistance

## Scope

Implement replay summarization assistance as an advisory service over existing replay timeline data.

## Design

- Add a `ReplayAdvisoryService` in `src/services/advisory/replay.py`.
- The service accepts a `ReplayTimeline`, operator question, persona/workspace context, and optional decision ID.
- It builds a TF-0065 `AdvisoryRequest` with replay timeline source references.
- It invokes `AIAdvisoryService`, which delegates to a provider.

## Boundaries

- No event-store dependency.
- No lifecycle transition dependency.
- No persistence.
- No concrete LLM adapter.
- No change to replay timeline construction.

## Validation

Tests should prove replay advisory output remains advisory, source-linked, and free of append/transition authority.

