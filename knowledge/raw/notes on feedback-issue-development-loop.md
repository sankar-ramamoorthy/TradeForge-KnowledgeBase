   a new loop is warranted, but for a specific reason: the current 12-phase loop is optimized for
   milestone feature work. It front-loads ADR evaluation and full KB stabilization cycles, which is right for planned
  features but creates unnecessary friction for feedback-originated fixes.

  The key structural differences a feedback/bug loop needs:

  1. Diagnosis phase before planning — the loop starts with "what is actually broken and why" rather than "pick next
  issue from roadmap"
  2. Lighter KB cycle — a small UX fix doesn't need raw → processed → topics → entities stabilization; a one-way
  processed note is enough
  3. ADR checkpoint is conditional — mandatory for TF-F002 (architectural lifecycle change), optional for TF-F001/F003
  (enhancements)
  4. Regression check — explicit step to verify the fix didn't break surrounding behaviour
  5. Feedback closure — close the loop back to the original test session that raised the issue

  The tradeoff is whether to have one unified feedback loop (covers bugs, enhancements, and architectural feedback
  issues alike, with conditional steps) or separate tracks per severity. I'd recommend one unified loop with conditional
   branches — simpler to maintain, and the TF-F classification field tells you which steps are mandatory.

  So: one new DOCS file (architectural rationale, parallel to runtime-kb-development-workflow.md) and one new KB
  playbook (operational loop, parallel to runtime-kb-development-loop.md).


  DOCS/feedback-issue-workflow.md (runtime repo) — the architectural rationale doc. Explains why the separate loop
  exists, the structural differences from the milestone loop, what TF-F### means, why diagnosis comes first, and when
  ADRs are required. Mirrors the style and role of runtime-kb-development-workflow.md.

  playbooks/development/feedback-issue-development-loop.md (KB repo) — the operational playbook. 12 phases with a
  conditional phase summary table so classification (bug / enhancement / architectural) immediately tells you which
  steps are mandatory. Key structural differences from the milestone loop:
  - Phase 2 is Diagnosis (not in the milestone loop at all)
  - Phase 3 ADR checkpoint is conditional on classification, not always mandatory
  - Phase 7 Regression Check is explicit and mandatory
  - Phase 9 is Feedback Loop Closure — closes against the original source observation
  - KB artifacts are lighter — one processed note rather than full stabilization cycles

  Both have full YAML frontmatter and Obsidian-style [[wiki links]] throughout.
