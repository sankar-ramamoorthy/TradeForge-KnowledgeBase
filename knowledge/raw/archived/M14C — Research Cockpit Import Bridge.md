
**M14C — Research Cockpit Import Bridge**

Purpose:

> Let TradeForge ingest operator-authored or AI-assisted research artifacts from a controlled local directory, convert them into draft lifecycle material, and require explicit user review before anything becomes canonical.

Core design:

```text
Research Cockpit / ChatGPT / Claude / Codex
        ↓
local import directory
        ↓
TradeForge import scanner
        ↓
parsed draft artifact
        ↓
Import Review workspace
        ↓
user edits / rejects / approves
        ↓
canonical lifecycle event
```

Important boundary:

The imported file is **not** a thesis.
It is a **draft thesis candidate** until the user approves it.

Initial import types:

* thesis draft
* plan draft
* catalyst note
* risk note
* invalidation condition
* evidence reference
* advisory observation
* watchlist candidate

Best first slice:

**TF-R001 — Import thesis draft from watched directory**

Acceptance criteria:

* Reads Markdown/YAML files from a configured import directory.
* Parses symbol, title, thesis narrative, catalysts, risks, invalidation conditions, evidence links.
* Creates a non-canonical `ImportedResearchDraft` or equivalent derived/import artifact.
* Does not create `TradeThesisCreated` automatically.
* Shows draft in an Import Review workspace.
* User can edit, reject, or promote.
* Promotion emits normal canonical lifecycle events.
* Import file gets moved to `/processed`, `/rejected`, or `/error`.
* Replay shows source provenance.

This fits your doctrine perfectly:

```text
external AI/research = advisory/source material
TradeForge = lifecycle authority
user = decision authority
event ledger = canonical truth
```

I would **not** let the directory importer directly create thesis/plan events. That would quietly violate the human-sovereignty boundary.

This bridge may become one of the most useful practical workflows in the whole product, because it lets you write freely in ChatGPT/Claude/Codex, then bring structured material into TradeForge without forcing TradeForge to become the writing environment.
