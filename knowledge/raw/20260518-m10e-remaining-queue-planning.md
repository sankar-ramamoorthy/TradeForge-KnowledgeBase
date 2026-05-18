---
title: 20260518 M10E remaining queue planning
type: raw-note
status: raw
---

# M10E Remaining Queue Planning

Issues: TF-F028, TF-F029, TF-F030, TF-F031, TF-F035, TF-F036.

Design intent:
- preserve Opportunity as a single-symbol cognitive workspace
- foreground instrument identity before interpretation
- translate operator-facing copy away from internal ontology terms
- make cognition-first synthesis primary and provenance secondary
- keep missing advisory context recovery-oriented rather than generic
- translate scenario UI into trader-facing conditional reasoning
- add advisory prompts for confirmation, invalidation, missing evidence, and judgment

Architecture boundaries:
- no lifecycle changes
- no canonical event changes
- no domain entity renames
- no hidden mutable truth; frontend-only continuity additions remain advisory
