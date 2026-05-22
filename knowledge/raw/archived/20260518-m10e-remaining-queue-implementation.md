---
title: 20260518 M10E remaining queue implementation
type: raw-note
status: raw
---

# M10E Remaining Queue Implementation

Completed:
- TF-F028 instrument identity banner for active setup evaluation
- TF-F029 trader-language replacement for candidate wording
- TF-F030 cognition-first evaluation surfaces with provenance demoted to secondary details
- TF-F031 recovery-oriented missing-context copy for price and fundamentals surfaces
- TF-F035 conditional-path trader language over canonical scenario events
- TF-F036 discretionary-thinking guidance prompts in Opportunity

Verification:
- npm.cmd run build
- uv run pytest tests\\test_workspace_routing.py tests\\test_workspace_state_contracts.py

Replay / lifecycle impact:
- no lifecycle authority changes
- no event schema changes
- Opportunity changes remain presentation-layer / advisory-layer refinements
