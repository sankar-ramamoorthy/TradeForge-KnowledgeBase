---
title: TF-R001 Local Thesis Import Dropoff Synthesis
type: processed-synthesis
status: processed
tags:
  - TradeForge
  - TF-R001
  - M14C
  - advisory-artifacts
  - thesis-workflow
  - operator-workflow
  - replayability
created: 2026-05-27
updated: 2026-05-27
source:
  - knowledge/raw/Implemented the missing operator dropoff side.md
  - knowledge/raw/archived/Implemented the missing operator dropoff side.md
  - knowledge/raw/feed back Local.md thesis import scan failed
  - knowledge/raw/20260527-tf-r001-scan-not-found-diagnosis.md
runtime_issue: TF-R001
runtime_milestone: M14C
---

# TF-R001 Local Thesis Import Dropoff Synthesis

## Summary

TF-R001 revealed a workflow gap between advisory artifact consumption and
operator-facing advisory artifact creation.

The initial implementation correctly added the consumer side:

- a read-only thesis import preview endpoint
- Thesis modal import preview controls
- selective field acceptance into editable Thesis draft fields
- provenance preservation on `decision.thesis_created`
- replay-visible advisory source context

However, operator testing expectations exposed that a human needs an obvious
dropoff path before any of those preview controls feel usable.

The stabilized implementation adds a minimal local import workflow:

```text
imports/incoming/*.md
    -> explicit Scan folder action
    -> deterministic markdown/front matter parsing
    -> non-canonical Advisory Artifact persistence
    -> Thesis modal preview
    -> operator-selected Thesis incorporation
    -> manual Develop Thesis submission
```

## Architectural Meaning

The distinction discovered here is important:

```text
import infrastructure
```

is not the same as:

```text
operator import workflow
```

The first is a substrate capability. The second is what the operator can
understand and use inside a [[Persona Workspace]].

For TradeForge, this means advisory artifact workflows need both:

- a durable non-canonical storage and query substrate
- an explicit operational handoff that makes source material available to the
  human at the point of decision formation

## Boundary Decision

TF-R001 remains inside the advisory boundary.

Local markdown dropoff:

- creates non-canonical [[Advisory Artifact]] records
- does not append lifecycle events
- does not create a [[Trade Idea]]
- does not create or revise a [[Thesis]]
- does not assign conviction
- does not approve plans
- does not create execution authority

Canonical lifecycle truth is still created only when the operator manually
submits the existing Thesis workflow, producing `decision.thesis_created`.

## Deterministic Contract

The local import contract is intentionally narrow.

Supported files:

```text
imports/incoming/*.md
```

Required front matter:

```yaml
---
artifact_role: thesis_draft
schema_version: thesis_draft.v1
symbol: ATKR
source: claude
---
```

Supported sections:

```markdown
# Thesis Narrative
# Catalysts
# Assumptions
# Invalidation Conditions
# Evidence Links
# Notes
```

The runtime does not infer thesis fields from arbitrary prose beyond those
deterministic sections.

## Replay Implication

Replay should show imported material only as advisory source context.

The canonical replay fact remains:

```text
decision.thesis_created
```

The import source appears through provenance such as:

```text
m14c_import_provenance
```

with explicit advisory/non-canonical labeling.

## Operational Synchronization Findings

This processing pass found and corrected runtime tracking drift:

- TF-R001 existed as a detailed issue but was missing from the issue index.
- Roadmap v2 had no M14C bridge between completed M14 and planned M15.
- TF-R001 scope initially described the preview/import consumer side but not
  the operator dropoff path clearly enough.

Runtime updates made during this pass:

- added TF-R001 to the issue index
- added M14C to `DOCS/Milestone_Roadmap_v2.md`
- updated TF-R001 with source traceability, implementation summary,
  verification, and remaining acceptance items

## Canonical Readiness

This synthesis is not yet a new canonical entity.

The stable concept is already covered by [[Advisory Artifact]], [[Thesis]],
[[Decision Lifecycle Engine]], and [[Persona Workspace]].

What is emerging is a reusable workflow heuristic:

> Advisory import features need an operator-visible dropoff surface, not only a
> downstream consumer panel.

This may later deserve a topic page if similar advisory workflows repeat across
Plan import, Replay annotation import, or Research Workspace handoff.

## Remaining Work

- Restart or otherwise align the running local API with the current TF-R001
  source tree before retesting. The current checkout exposes
  `POST /advisory/thesis-imports/scan-local`, and the targeted regression test
  passes, but the already-running API at `127.0.0.1:8000` returned
  `{"detail":"Not Found"}` for that route during manual feedback processing.
- Complete operator manual test of the ATKR drop-folder workflow against the
  aligned backend.
- Update runtime README after test feedback.
- Consider whether a future dedicated workflow document should describe local
  advisory markdown imports across more artifact roles.

## Manual Test Feedback: Scan Route Not Found

Source feedback captured on 2026-05-27 reported:

```text
Local thesis import scan failed: Not Found
```

The raw note also showed `imports/incoming/ATKR_thesis_draft.md` present in the
runtime checkout.

Processing distinguished two facts:

- the current source tree contains the scan route and passes the targeted local
  scan regression test
- the already-running local API at `127.0.0.1:8000` returned FastAPI `404 Not
  Found` for the same route

The stabilized interpretation is operational synchronization drift rather than
a new semantic or architectural gap. TF-R001 remains open until the operator
manual test is rerun against a backend instance aligned with the current source.
