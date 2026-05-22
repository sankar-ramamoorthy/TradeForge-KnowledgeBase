---
title: TF-F006 Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-16
source:
  - knowledge/raw/20260516-tf-f006-planning.md
issue: TF-F006
milestone: M10C
---

# TF-F006 Planning Synthesis

TF-F006 is a composition-root refactor, not a provider-domain redesign.

## Durable Conclusions

- Provider adapters should remain constructor-driven and security-agnostic.
- Credential delivery belongs only in the app composition root.
- A provider-construction registry inside the composition root is the lowest-complexity seam that keeps current and future credentialed providers on one path.
- Keyless providers and credentialed providers can coexist without weakening the boundary when provider selection is explicit.

## Canonicalization Assessment

No additional canonical promotion is required. ADR-0037 remains sufficient architectural authority.
