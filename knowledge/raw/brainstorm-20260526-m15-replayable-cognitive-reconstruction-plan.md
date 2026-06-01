---
title: M15 Replayable Cognitive Reconstruction Plan And Handoff
type: brainstorm
status: raw
tags: [tradeforge, m15, replayable-cognition, cognitive-reconstruction, handoff]
created: 2026-05-26
---

# M15 Replayable Cognitive Reconstruction Plan And Handoff

## Trigger

User requested a detailed plan for M15 and a handoff captured in the KB raw
folder because the current Codex session is running short of tokens.

## Raw Input

> please make a detailed plan for M15 and capture it as a note in the KB Raw
> folder alongwith a handoff.
> C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\knowledge\raw
> running short of tokens.

## Context Snapshot

Runtime repository:

- Path: `C:\Users\bosto\dockerstuff\TradeForge`
- Runtime branch after M14 completion: `feature/m14-behavioral-intelligence-completion`
- M14 runtime commit: `b58aab4` - `Complete M14 behavioral intelligence projections`
- Runtime worktree was clean after M14 commit.

Knowledge-base repository:

- Path: `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge`
- KB branch after M14 capture: `M14`
- M14 KB commit: `f6e72fb` - `Capture M14 behavioral intelligence completion`
- KB worktree was clean before this note.

M15 roadmap entry:

- Milestone: `M15 - Replayable Cognitive Reconstruction`
- Status in roadmap at planning time: `Planned`
- Semantic intent: reconstruct historical discretionary cognition with
  high-fidelity replayable context.
- Architectural significance: replay evolves from what happened toward what
  was believed, why it was believed, what evidence existed, and what was
  ignored.

Linked roadmap issues:

- `TF-D001`: Implement point-in-time cognition reconstruction
- `TF-D002`: Implement advisory state replay snapshots
- `TF-D003`: Implement historical evidence reconstruction
- `TF-D004`: Implement conviction evolution timelines
- `TF-D005`: Implement ignored-evidence overlays
- `TF-D006`: Implement contextual replay comparisons
- `TF-D007`: Implement replay-linked interpretation history
- `TF-D008`: Implement replay-attached uncertainty state

Important gap:

- `TF-D001` through `TF-D008` appear in the roadmap but were not found in the
  runtime `DOCS/ISSUE_REGISTER.md` during this planning pass.
- Before implementation, issue discipline requires adding full issue-register
  records with milestone, affected layers, ADRs, invariants, scope, acceptance
  criteria, and out-of-scope boundaries.

## Dominant Task Category

Replay / Historical Reconstruction Work, with limited overlap into advisory
artifact replay and cognitive UX surfaces.

Relevant invariants:

- Event Ledger Canonical Truth
- Events Are Immutable
- Replayability Is Foundational
- Historical Integrity
- Derived State Must Remain Distinguishable
- AI Advisory Boundary
- Human Decision Sovereignty
- Reflection And Review Are First-Class

Primary architectural rule:

M15 must be a derived reconstruction layer. It must not write canonical events,
mutate lifecycle state, call live market/advisory APIs during replay, or infer
operator beliefs as canonical fact.

## M15 Design Thesis

M15 should create a `CognitiveReconstruction` read model that composes existing
event-backed and advisory-capture history into point-in-time cognitive state.

The model should answer:

- What did the operator believe at this point?
- What thesis version, plan, assumptions, branches, annotations, review
  reflections, advisory observations, interpretations, market snapshots, and
  behavioral signals were visible?
- What evidence was available but not reflected in the active thesis or plan?
- How did conviction and uncertainty evolve over time?
- Which conclusions are canonical facts, derived reconstruction, advisory
  artifacts, or inferred review context?

The reusable data flow should be:

```text
Event Ledger history
  -> replay timeline / historical reconstruction
  -> cognitive artifacts at timestamp
  -> advisory and market snapshots captured before timestamp
  -> behavioral signals derived from source history
  -> cognitive reconstruction read model
  -> replay and review surfaces
```

## Architectural Boundaries

Must preserve:

- Event ledger remains canonical truth.
- Cognitive reconstruction is derived and rebuildable.
- Advisory observations and interpretations remain advisory, not truth.
- Market snapshots are historical/advisory context, not current live data.
- Behavioral signals remain deterministic derived review projections.
- AI may summarize reconstruction later, but cannot generate reconstruction
  facts or mutate lifecycle state.

Must avoid:

- New hidden state stores for "belief."
- Live provider calls during replay reconstruction.
- Treating ignored evidence as operator intent unless explicit source links
  justify the label.
- Turning decision-quality or cognitive metrics into approval gates.
- Creating dashboard-style analytics instead of replay-centered inspection.

## Proposed Runtime Architecture

### Domain Layer

Add or extend pure domain read-model types:

- `CognitiveReconstruction`
- `CognitiveReconstructionView`
- `CognitiveArtifactSnapshot`
- `EvidenceSnapshot`
- `ConvictionTimeline`
- `IgnoredEvidenceOverlay`
- `ContextualReplayComparison`
- `ReplayInterpretationHistory`
- `ReplayUncertaintyState`

All domain types should include:

- `authority`
- `is_canonical`
- `source_event_refs`
- source artifact IDs where applicable
- timestamp bounds
- decision/persona/workspace identity

### Services Layer

Add a read-only service such as:

- `CognitiveReconstructionReadService`

Responsibilities:

- Read immutable event history.
- Filter by decision/persona/workspace and optional `at` timestamp.
- Compose existing replay timeline and cognitive snapshot behavior.
- Query advisory stores for historical observations/interpretations captured
  before `at`.
- Query persisted market snapshots only; no live market fetches.
- Compose behavioral signals from event history up to `at`.
- Return derived reconstruction view.

No service method should append events.

### App/API Layer

Add read endpoints only:

- `GET /replay/cognitive-reconstruction`
- `GET /replay/cognitive-reconstruction/{decision_id}`
- `GET /replay/cognitive-reconstruction/{decision_id}/evidence`
- `GET /replay/cognitive-reconstruction/{decision_id}/conviction-timeline`
- `GET /replay/cognitive-reconstruction/{decision_id}/ignored-evidence`
- `GET /replay/cognitive-reconstruction/{decision_id}/comparisons`
- `GET /replay/cognitive-reconstruction/{decision_id}/interpretation-history`
- `GET /replay/cognitive-reconstruction/{decision_id}/uncertainty`

Query parameters should include:

- `persona_id`
- `workspace_id`
- `at`
- optional comparison timestamp for TF-D006

### Frontend Layer

Extend Replay Workspace rather than adding a new dashboard:

- Point-in-time cognition panel.
- Evidence available at selected timeline moment.
- Conviction evolution strip.
- Ignored evidence overlay attached to timeline entries.
- Contextual comparison between selected timestamp and current/another
  timestamp.
- Interpretation history panel.
- Uncertainty state panel.

All surfaces should label authority:

- canonical facts
- derived reconstruction
- advisory artifact
- inferred review context

## Proposed Issue Plan

### Prerequisite: Register TF-D001 Through TF-D008

Before implementation, update `DOCS/ISSUE_REGISTER.md` with full issue records
for all M15 issues.

Each issue should include:

- Status: `Planned`
- Milestone: `M15`
- Branch
- Affected Layer
- Linked ADRs
- Impacted Invariants
- Problem
- Scope
- Acceptance Criteria
- Out Of Scope
- ADR checkpoint

Likely ADRs:

- ADR-0001 Event Sourcing Core Model
- ADR-0002 Decision Lifecycle Engine
- ADR-0008 Replay System Design
- ADR-0014 Replay-Centric UX Model
- ADR-0033 Structured Cognitive Artifact Model
- ADR-0035 Replay Cognitive Reconstruction Strategy
- ADR-0041 Advisory Observation And Cognitive Evidence Foundation
- ADR-0042 Contextual Interpretation And Thesis Influence
- ADR-0044 Behavioral Signal Derived Read Model

ADR checkpoint:

- A new M15 ADR may be warranted if `CognitiveReconstruction` becomes a stable
  first-class derived model with cross-domain composition rules. The ADR should
  not add event types unless a later issue explicitly records review events.

### TF-D001: Implement Point-In-Time Cognition Reconstruction

Goal:

Create the base reconstruction service for what the operator could know at a
given timestamp.

Scope:

- Compose thesis, thesis revisions, plan, plan revisions, scenario branches,
  annotations, lifecycle stage, replay timeline state, and behavioral signals
  up to `at`.
- Return derived reconstruction metadata and source event references.
- Support decision/persona/workspace filters.

Acceptance:

- Reconstruction at timestamp excludes future events.
- Reconstruction does not use live APIs.
- Output is `authority: derived`, `is_canonical: false`.
- Source event references identify every included cognitive artifact.

Out of scope:

- Advisory evidence snapshots.
- Ignored evidence analysis.
- Frontend polish beyond basic API validation unless needed for review.

### TF-D002: Implement Advisory State Replay Snapshots

Goal:

Attach captured advisory observations, candidates, artifacts, and
interpretations that existed at the replay timestamp.

Scope:

- Query advisory stores by captured timestamp.
- Include artifact IDs, observation IDs, interpretation IDs, provenance, and
  uncertainty metadata.
- Preserve advisory authority labels.

Acceptance:

- Advisory state is replay-safe and does not call the current advisory
  provider.
- Advisory content remains non-canonical.
- Missing advisory store content degrades gracefully.

Out of scope:

- AI generation during replay.
- Promotion of advisory content to lifecycle events.

### TF-D003: Implement Historical Evidence Reconstruction

Goal:

Reconstruct evidence visible to the operator at a point in time.

Scope:

- Include thesis evidence, advisory evidence, market snapshot references,
  interpretation evidence, and review references captured before `at`.
- Deduplicate evidence by source kind/source ID/artifact ID.
- Preserve freshness/staleness as timestamp-derived context, not truth.

Acceptance:

- Evidence list is source-linked.
- Evidence availability is time-bounded.
- Evidence state is distinguishable from interpreted conclusions.

Out of scope:

- Live evidence fetching.
- AI evidence scoring.

### TF-D004: Implement Conviction Evolution Timelines

Goal:

Show how conviction changed across thesis revisions, interpretations, reviews,
and behavioral context.

Scope:

- Build a chronological timeline from thesis confidence, interpretation
  confidence ranges, review decision quality, and behavioral signals.
- Keep each track separate rather than collapsing into one score.

Acceptance:

- Timeline is deterministic.
- Conviction, uncertainty, and review quality remain separate dimensions.
- Source references are present for every entry.

Out of scope:

- Predictive confidence scoring.
- Approval gating.

### TF-D005: Implement Ignored-Evidence Overlays

Goal:

Surface evidence available at the time that appears absent from later thesis,
plan, or review artifacts.

Scope:

- Compare evidence summaries/tags/source references against thesis narrative,
  assumptions, invalidation conditions, plan assumptions, and review text.
- Label results as "potentially unaddressed evidence," not proof of operator
  intent.
- Include source evidence and compared artifact references.

Acceptance:

- Overlay uses explicit deterministic matching rules.
- Output avoids claiming emotional or cognitive intent as fact.
- Results are replay context, not discipline violations by themselves.

Out of scope:

- NLP/AI-only ignored-evidence inference.
- Canonical mistake recording.

### TF-D006: Implement Contextual Replay Comparisons

Goal:

Compare cognitive reconstruction across two timestamps.

Scope:

- Compare thesis/plan/branch/evidence/advisory/behavioral state at `left_at`
  and `right_at`.
- Identify added, removed, changed, and unchanged cognitive context.
- Use stable IDs and source references where possible.

Acceptance:

- Comparison is deterministic and replay-safe.
- No mutable UI state is required.
- Differences preserve authority labels.

Out of scope:

- Outcome hindsight scoring.
- Live market comparison.

### TF-D007: Implement Replay-Linked Interpretation History

Goal:

Show advisory interpretation history in replay context.

Scope:

- Chronologically list interpretations captured before replay timestamp.
- Include thesis influence, contextual weight, confidence range, conflict
  markers, source observation IDs, and provenance.
- Link entries to replay timeline and cognitive reconstruction panels.

Acceptance:

- Interpretation history reconstructs without current AI output.
- Advisory/non-canonical status is explicit.
- Links to source observations are inspectable.

Out of scope:

- Generating new interpretations during replay.
- Treating interpretations as canonical decision state.

### TF-D008: Implement Replay-Attached Uncertainty State

Goal:

Make uncertainty visible as part of cognitive replay.

Scope:

- Aggregate uncertainty bands, confidence ranges, caveats, missing evidence,
  stale evidence, and conflicting interpretations at timestamp.
- Return a structured uncertainty state with source references.
- Display in replay as context, not as a score.

Acceptance:

- Uncertainty state is derived/advisory as appropriate.
- Missing and conflicting context is visible.
- The model separates uncertainty from decision quality and outcome.

Out of scope:

- AI risk scoring.
- Automated recommendations.

## Recommended Order

1. Register TF-D001 through TF-D008 in `DOCS/ISSUE_REGISTER.md`.
2. Create or update an ADR if `CognitiveReconstruction` is promoted as a
   first-class derived model.
3. Implement TF-D001 first as the base composition service and API.
4. Add TF-D002 and TF-D003 to bring advisory/evidence state into the
   reconstruction.
5. Add TF-D004 and TF-D008 for evolution and uncertainty tracks.
6. Add TF-D005 after evidence reconstruction exists.
7. Add TF-D006 once two point-in-time reconstructions are reliable.
8. Add TF-D007 to make interpretation history replay-native.
9. Add frontend surfaces incrementally, preferably after each backend read
   model is stable.
10. Synchronize roadmap and issue register after each issue.

## Testing Strategy

Backend:

- Unit tests for point-in-time filtering.
- Tests proving future events are excluded.
- Tests proving no event-store writes occur.
- Tests for advisory state reconstruction with captured timestamps.
- Tests for ignored-evidence matching rules.
- Tests for comparison symmetry and stable IDs.
- Tests for uncertainty aggregation.

Frontend:

- Typecheck for API types.
- Build verification.
- Component tests if project conventions support them.
- Manual replay walkthrough for timestamp selection and authority labeling.

Regression checks:

- `uv run pytest tests/test_replay_timeline.py tests/test_replay_timeline_service.py`
- New M15 test module, likely `tests/test_cognitive_reconstruction.py`
- Advisory interpretation tests where stores are involved.
- `npm.cmd run typecheck`
- `npm.cmd run build`

## Documentation And Operational Sync

Runtime docs to update:

- `DOCS/ISSUE_REGISTER.md`
- `DOCS/Milestone_Roadmap_v2.md`
- New ADR if warranted.

KB captures:

- Planning raw note: this file.
- Per-issue implementation captures as work proceeds.
- Processed synthesis after M15 completion.

## Handoff

Next Codex session should begin with:

1. Read repository `AGENTS.md`.
2. Load required semantic bootstrap:
   - `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\SEMANTIC_BOOTSTRAP.md`
   - `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\INVARIANTS.md`
   - `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\knowledge\index\runtime-context-map.md`
3. Load this raw note:
   - `C:\Users\bosto\dockerstuff\knowledge-base\TradeForge\knowledge\raw\brainstorm-20260526-m15-replayable-cognitive-reconstruction-plan.md`
4. Confirm runtime branch/state:
   - Runtime repo clean after M14 commit `b58aab4`.
   - KB repo clean after M14 commit `f6e72fb`, except this note if not yet
     committed.
5. Inspect `DOCS/ISSUE_REGISTER.md` for whether TF-D001 through TF-D008 have
   been registered since this note.
6. If not registered, start with a planning/docs issue to add M15 issue
   records before runtime code.
7. Keep dominant context category as Replay / Historical Reconstruction.
8. Do not implement M15 code until issue discipline is satisfied.

Recommended first concrete task:

> Add full issue-register records for TF-D001 through TF-D008 and evaluate
> whether an ADR is required for `CognitiveReconstruction` as a first-class
> derived replay model.

## Questions

- Should `CognitiveReconstruction` become a named canonical glossary term
  before implementation?
- Should M15 get a new ADR, or does ADR-0035 already cover the derived model
  boundary sufficiently?
- Should advisory stores expose timestamp-bounded query APIs first, or should
  the reconstruction service filter in memory for the initial slice?
- Should ignored evidence be implemented with deterministic text matching only
  in M15, leaving AI-assisted analysis to a later advisory milestone?

## Concerns

- M15 crosses replay, advisory artifacts, market snapshots, behavioral signals,
  and frontend surfaces. Scope control is critical.
- The phrase "what was believed" can drift into inferred operator truth. The
  runtime must instead reconstruct recorded beliefs and visible context.
- Ignored-evidence overlays are especially sensitive; they must be framed as
  "potentially unaddressed evidence" unless explicit review artifacts say it
  was ignored.
- Replay must not use live APIs or current model output.

## Possible Next Outputs

- Issue-register update for TF-D001 through TF-D008.
- ADR candidate for Cognitive Reconstruction Derived Model.
- Runtime implementation plan for TF-D001.
- Processed KB synthesis after M15 architecture is accepted.
