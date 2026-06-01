• Implemented the missing operator dropoff side.

  Now the intended ATKR flow is:

  1. Put a markdown file here:
     C:\Users\bosto\dockerstuff\TradeForge\imports\incoming\ATKR_thesis_draft.md
  2. Use front matter plus sections like:

  ---
  artifact_role: thesis_draft
  schema_version: thesis_draft.v1
  symbol: ATKR
  source: claude
  ---

  # Thesis Narrative

  ATKR may benefit from infrastructure spending and improving construction demand.

  # Catalysts

  - infrastructure backlog expansion
  - margin recovery

  # Assumptions

  - nonresidential demand remains stable

  # Invalidation Conditions

  - backlog deterioration
  - margin compression resumes

  3. In the Thesis modal, click Scan folder in the Import Preview panel.
  4. TradeForge parses the markdown, persists it as a non-canonical advisory artifact, refreshes the preview list, and then you can accept fields into the
     editable thesis draft.
  5. Nothing canonical happens until the operator manually clicks Develop Thesis.

  Added:

  - POST /advisory/thesis-imports/scan-local
  - deterministic markdown/front matter parser for imports/incoming/*.md
  - UI scan button and drop-folder cue
  - regression test for local ATKR markdown import
  - TF-R001 issue scope updated to include this on-demand local drop workflow

  Verified:

  - uv run pytest tests\test_advisory_artifact.py tests\test_develop_thesis_workflow.py
  - uv run mypy src\app\api\routes.py tests\test_advisory_artifact.py
  - uv run ruff check src\app\api\routes.py tests\test_advisory_artifact.py --select F
  - npm.cmd run typecheck
  - npm.cmd run build
  - git diff --check

  One note: full E501 lint on routes.py still reports existing long-line debt unrelated to this change.