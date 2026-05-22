---
title: TF-F007 Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-16
source:
  - knowledge/raw/20260516-tf-f007-planning.md
issue: TF-F007
milestone: M10C
---

# TF-F007 Planning Synthesis

TF-F007 is the operator-translation step for the accepted credential boundary.

## Durable Conclusions

- Credential setup guidance belongs at the repo entry level because operators need it before starting credentialed providers.
- The guide must mirror the actual runtime command surface rather than describing aspirational workflows.
- `TRADEFORGE_MASTER_KEY` needs an explicit ignore guard even though the canonical path is environment-only, because accidental file creation is an operational risk worth blocking.

## Canonicalization Assessment

No additional canonical doctrine promotion is required. ADR-0037 remains the governing artifact.
