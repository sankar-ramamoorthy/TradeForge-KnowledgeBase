---
title: TF-R002 Plan Import Mediation Implementation Capture
type: implementation-capture
status: raw
created: 2026-05-28
tags:
  - TradeForge
  - TF-R002
  - M14C
  - implementation
  - plan-import
  - advisory-boundary
  - replayability
---

# TF-R002 Plan Import Mediation Implementation Capture

## Summary

Implemented TF-R002 as a narrow Plan Workspace import mediation slice.

The implementation mirrors the TF-R001 advisory import preview model but applies
stricter execution-boundary constraints because Plan authoring sits next to
approval and execution.

## Runtime Changes

- Added read-only `GET /advisory/plan-imports`.
- Added on-demand `POST /advisory/plan-imports/scan-local`.
- Added deterministic `plan_draft.v1` mapped fields:
  - `entry_rationale`
  - `stop_rationale`
  - `target_rationale`
  - `risk_notes`
- Rejected/ignored prohibited mapped authority such as sizing, prices,
  quantities, broker orders, approval, and execution instructions.
- Extended `POST /lifecycle/decisions/create-plan` with optional import
  provenance.
- Preserved provenance under `m14c_import_provenance` on the normal
  `decision.plan_created` event payload.
- Updated Plan Development UI with advisory import preview, local scan,
  accept/reject, edited markers, and provenance submission.
- Preserved `sizing_rationale` as manually authored.
- Updated Replay UI to label provenance as
  `Advisory source, operator-authored plan`.

## Boundary Decisions

- No new canonical event type was introduced.
- Import preview does not append events.
- Field acceptance before submission remains draft UI state.
- Advisory artifacts remain non-canonical.
- The only canonical plan fact remains `decision.plan_created`.
- Plan import has no sizing, approval, execution, or broker authority.

## Verification

- `uv run pytest tests\test_advisory_artifact.py tests\test_create_plan_workflow.py tests\test_develop_thesis_workflow.py`
- `uv run mypy src\app\api\routes.py tests\test_advisory_artifact.py tests\test_create_plan_workflow.py`
- `uv run ruff check src\app\api\routes.py tests\test_advisory_artifact.py tests\test_create_plan_workflow.py --select F`
- `npm.cmd run typecheck`
- `npm.cmd run build`
- `git diff --check`

## Follow-Up

- Manual operator test should validate the local plan markdown dropoff workflow
  against a running backend.
- If thesis and plan import mediation continue to hold, prepare an ADR for the
  broader selective import mediation pattern.
