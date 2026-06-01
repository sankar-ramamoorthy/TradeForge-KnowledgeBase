---
title: TF-F077 Feedback - Indented Thesis Front Matter
type: raw-feedback
status: raw
tags: [TradeForge, TF-F077, feedback, thesis-import, local-import]
created: 2026-05-28
updated: 2026-05-28
issues:
  - TF-F077
---

# Source Feedback

The operator reported that the import preview scanned two files and imported
zero, then clarified that the relevant file was `ATKR_thesis_draft.md`.

Observed local file shape:

```markdown
 ---
  artifact_role: thesis_draft
  schema_version: thesis_draft.v1
  symbol: ATKR
  source: claude
  ---

  # Thesis Narrative

  ATKR may benefit from infrastructure spending and improving construction demand.
```

# Initial Observation

The thesis draft carries the expected role, schema version, symbol, source, and
markdown thesis sections. The visible anomaly is leading whitespace before the
front matter delimiters and front matter keys.
