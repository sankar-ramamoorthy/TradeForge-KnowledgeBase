---
title: TF-F005 Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-16
source:
  - knowledge/raw/20260516-tf-f005-planning.md
issue: TF-F005
milestone: M10C
---

# TF-F005 Planning Synthesis

TF-F005 is the operationalization step for the credential boundary established by ADR-0037 and TF-F004.

## Durable Conclusions

- Fernet-backed local encryption is the chosen M10C mechanism because it satisfies the local-first scope without introducing external infrastructure.
- `.keys.enc` should store encrypted credential payloads plus explicit metadata; plaintext provider values must not be written to disk.
- A minimal command surface is justified because the issue explicitly requires both master-key generation and initial credential registration.
- Composition-root provider wiring remains deferred to TF-F006 and must not leak into this issue.

## Canonicalization Assessment

No additional canonical doctrine promotion is required. ADR-0037 remains sufficient authority for the runtime work.
