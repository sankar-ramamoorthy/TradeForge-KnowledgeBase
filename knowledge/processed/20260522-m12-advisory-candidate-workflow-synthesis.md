---
title: M12 Advisory Candidate Workflow Synthesis
type: processed-synthesis
status: processed
created: 2026-05-22
source_history:
  - knowledge/raw/archived/20260522-m12-advisory-candidate-workflow.md
tags:
  - TradeForge
  - M12
  - advisory-candidates
  - lifecycle-boundary
---

# M12 Advisory Candidate Workflow Synthesis

## Stabilized Observation

Advisory candidates are best treated as reviewable advisory artifacts rather
than lifecycle-adjacent decision objects. They can be durable, queryable, and
visible in operator workflows while remaining outside commitment space.

## Boundary

Candidate ingestion belongs to advisory space:

```text
Observation -> Candidate -> Review Queue
```

Operator promotion enters commitment space only through the existing lifecycle
workflow:

```text
New Trade Idea -> Idea
```

The bridge between these spaces is explicit operator action, editable prefilled
context, and traceability to the advisory candidate. Advisory candidate content
does not become canonical truth.

## Durable Rule

Candidate review queues should remain derived and deterministic unless a later
ADR explicitly introduces persisted operator queue actions. Hidden AI ranking,
automatic lifecycle promotion, and candidate-to-trade shortcuts remain outside
M12 authority.

## Runtime Evidence

Runtime implementation for TF-A011 through TF-A013 uses the existing advisory
observation artifact store and `advisory.observation_captured` event fact.
Promotion traceability is recorded on the operator-owned lifecycle event without
granting advisory artifacts lifecycle authority.
