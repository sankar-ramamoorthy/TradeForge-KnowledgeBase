---
title: TF-F004 Planning Synthesis
type: processed-synthesis
status: processed
created: 2026-05-16
source:
  - knowledge/raw/20260516-tf-f004-planning.md
issue: TF-F004
milestone: M10C
---

# TF-F004 Planning Synthesis

TF-F004 confirms an already-stabilized architectural decision rather than introducing a new semantic concept.

## Durable Conclusions

- `src/security/` is an intentional top-level runtime boundary, not an infrastructure subfolder.
- `Credential` is a pure, immutable domain model for provider capability metadata.
- Storage, decryption, and provider wiring remain deliberately deferred to TF-F005 through TF-F007.
- Replay-oriented fields belong on the model now so future credential-status history can be represented without changing the core entity shape.
- Provider adapters must remain independent from credential storage and decryption concerns.

## Canonicalization Assessment

No additional canonical KB promotion is required during TF-F004 planning because ADR-0037 already stabilizes the durable decision. The implementation should therefore remain bounded to the runtime model and tests.

## Follow-On Dependencies

- TF-F005: KeyManager and encrypted local credential store
- TF-F006: composition-root wiring through CredentialStore
- TF-F007: operator setup and rotation documentation
