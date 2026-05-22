---
title: TF-0067 Planning - Review Assistance
type: raw-planning
status: raw
created: 2026-05-19
issue: TF-0067
milestone: M11
---

# TF-0067 Planning - Review Assistance

## Scope

Implement review assistance as an advisory service over structured review reflection artifacts.

## Design

- Add `ReviewAdvisoryService` in `src/services/advisory/review.py`.
- Accept a `ReviewReflectionArtifact`, operator question, and operational context.
- Build a TF-0065 `AdvisoryRequest` with a review-artifact source reference.
- Delegate generation through `AIAdvisoryService`.

## Boundaries

- No review event mutation.
- No lifecycle transition or review completion authority.
- No persistence.
- No behavioral scoring or discipline engine.
- No concrete LLM adapter.

