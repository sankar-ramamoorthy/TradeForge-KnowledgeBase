 I have enough evidence to separate the work cleanly. The strongest structure is one runtime-facing feedback issue for
  the workstation-layout gap, plus one documentation/doctrine issue to formally connect the new middle design layer to
  the existing design stack so future frontend work does not drift back toward centered shells.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• Findings

  1. The primary gap is real and concrete: current workspaces still render like a centered document shell, not like
     operational workstations. The screenshot and runtime CSS both show the same cause:
      - frontend/DESIGN.md defines shell_max_width: "1220px".
      - styles.css applies .app-shell { width: min(var(--layout-shell-max), calc(100vw - var(--space-page-inline)));
        margin: 0 auto; }.
      - WorkspaceLayout only supports sidebar + main, with no persistent contextual awareness rail.
      - The Operating Workspace content is still largely stacked vertically rather than composed as parallel operational
        panels.
        This conflicts with UX_DOCTRINE.md, WORKSPACES.md, and the new draft design documents, all of which point toward
        parallel cognition, persistent context, and workstation use of width. Relevant files: C:/Users/bosto/
        dockerstuff/TradeForge/frontend/src/styles.css, C:/Users/bosto/dockerstuff/TradeForge/frontend/src/
        operationalLayout.tsx, design/workspace-operational-layouts.md.
  2. There is also a documentation-layer gap: the design stack is now effectively three-layered, but that relationship
     is not yet fully normalized in the knowledge base/runtime translation boundary.
      - Existing note 20260515-frontend-design-md-role-and-usage.md still describes a two-layer model.
      - The new draft docs correctly introduce the missing middle layer: operational workspace architecture.
      - The recommended section addition captures the correction, but it has not yet been integrated into the original
        note or promoted into a stable design relationship model.
        This is smaller than the runtime gap, but important because it explains why frontend implementation drifted
        toward a visually coherent but semantically weaker layout model. Relevant files: knowledge/raw/20260515-
        frontend-design-md-role-and-usage.md, knowledge/raw/recomnended section addition to 20260515-frontend-design-md-
        role-and-usage.md.
  3. I found the screenshot asset feedback on screen realestate Screenshot 2026-05-16 120442.png, but not a Markdown
     file named feedback on screen realestate Screenshot 2026-05-16 120442.md. The substantive written feedback appears
     to be in 2026-05-16 feed back on screen real estate and design.md.

  Recommended Issue Set

  1. TF-F012
     Title: Replace centered workspace shell with workstation-oriented operational layout model
     Classification: enhancement
     Source: 2026-05-16 feed back on screen real estate and design.md plus screenshot from 2026-05-16
     Observable gap: desktop workspaces waste horizontal real estate, hide parallel state in vertical stacks, and lack a
     persistent contextual awareness zone.
     Root cause: frontend runtime translation still encodes a centered max-width shell and a two-zone layout primitive
     that no longer matches the evolved workspace doctrine.
     Minimum correct scope:
      - remove centered-shell assumptions for operational workspaces
      - introduce a desktop three-zone composition model: nav, primary operational surface, context rail
      - adapt the Operating Workspace first as the proving surface
      - preserve canonical/advisory visual separation
        Out of scope:
      - resizable panels
      - multi-monitor support
      - full redesign of every workspace in one pass
      - mobile optimization beyond safe responsive degradation
        ADR checkpoint: likely optional. This fits existing UX/workspace architecture rather than changing ontology or
        lifecycle semantics.
  2. TF-F013
     Title: Formalize the three-layer design architecture between doctrine, workspace composition, and frontend
     translation
     Classification: doctrine
     Source: 20260515-frontend-design-md-role-and-usage.md, the recommended addition note, and the new draft design/
     documents
     Observable gap: frontend design documentation currently explains tokens and visual translation, but the now-
     required operational workspace layer is not yet formally integrated into the design hierarchy.
     Root cause: the system evolved from two design layers to three, and the documentation has not caught up.
     Minimum correct scope:
      - update the raw frontend-design note with the three-layer model
      - decide whether workspace-operational-layouts.md, workspace-zoning-reference.md, and operational-panel-
        patterns.md remain draft or should enter a promotion path
      - ensure frontend/DESIGN.md is explicitly positioned as translation, not owner, of operational composition
        semantics
        Out of scope:
      - changing runtime code
      - canonicalizing every draft design artifact immediately
        ADR checkpoint: not needed unless you decide to make a durable architectural rule change beyond documentation.

  Concrete Plan Using The Feedback Loop

  1. Phase 1, triage
     Register TF-F012 and TF-F013 in the runtime issue register when you are ready to schedule them. Use the raw note
     and screenshot as mandatory source material.
  2. Phase 2, diagnosis
     Create:
      - knowledge/raw/20260516-tf-f012-diagnosis.md
      - knowledge/raw/20260516-tf-f013-diagnosis.md
        TF-F012 diagnosis should explicitly cite the current shell_max_width, centered .app-shell, lack of context rail,
        and vertical-stack composition.
        TF-F013 diagnosis should capture the design-stack drift from two layers to three.
  3. Phase 3, ADR checkpoint
      - TF-F012: probably no ADR unless you decide the workstation zoning model must become a durable runtime
        architecture rule across all future workspaces.
      - TF-F013: no ADR expected.
      - update layout primitives first
      - convert Operating Workspace as the reference implementation
      - validate that advisory context remains visually non-authoritative
      - only then propagate the pattern to Opportunity, Plan Review, Active Position, Replay, and Review workspaces in
        later bounded issues if needed
        For TF-F013, fold the recommended hierarchy section into the frontend-design raw note and decide what promotion
        path the three new draft design files should follow.
  5. Phase 5, planning capture
     Create:
      - knowledge/raw/20260516-tf-f012-planning.md
      - knowledge/raw/20260516-tf-f013-planning.md

  My recommendation

  Treat TF-F012 as the main issue and TF-F013 as the supporting doctrine issue. Do not split TF-F012 into many small
  tickets yet. The current evidence points to one root problem with several symptoms:

  - dead horizontal space
  - absent right context rail
  - stacked instead of parallel surfaces
  - shell behavior still optimized for document reading

  Once TF-F012 is diagnosed, implementation can be split later only if the work proves too large for one bounded fix.


› i also added a screenshot in raw "feedback on screen realestate Screenshot 2026-05-16 120442.png"


• I found and reviewed that screenshot during the analysis. It directly supports TF-F012:

  - large unused left/right desktop space
  - narrow centered operational surface
  - no persistent right contextual awareness rail
  - active decisions compressed into a vertical list
  - layout behaving more like a centered app shell than an operational workstation

  That screenshot should be listed as source evidence for TF-F012 alongside knowledge/raw/2026-05-16 feed back on screen
  real estate and design.md.
