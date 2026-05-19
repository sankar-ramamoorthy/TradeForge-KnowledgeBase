---
title: Opportunity Workspace UX And Context Acquisition Gaps
type: brainstorm
status: raw
tags: [tradeforge, brainstorm, opportunity-workspace, ux, cognition, context-acquisition, trader-language]
created: 2026-05-17
---

# Opportunity Workspace UX And Context Acquisition Gaps

## Trigger

A focused architecture and UX discovery session on the current Opportunity Workspace, using live runtime screenshots and operator reactions to identify where event-sourced implementation semantics are leaking into discretionary trading cognition UX.

## Raw Input

Primary source session:

- `knowledge/raw/archived/Brainstorm session 20260517 on the Opportunity Workspace.md`

Referenced screenshots:

- `knowledge/raw/brainstorm session Screenshot 2026-05-17 135450.png`
- `knowledge/raw/brainstorm session Screenshot 2026-05-17 142020.png`

The full raw transcript remains preserved in the source note above. Core themes raised in that session included:

- The Opportunity Workspace currently exposes implementation categories more strongly than trader cognition.
- Internal ontology terms such as `candidate`, `scenario branches`, `canonical`, `derived`, and `advisory` are appearing too directly in operator-facing surfaces.
- The workspace lacks a strong instrument identity anchor once the trader enters a selected symbol flow.
- Missing-data states such as `Fundamentals unavailable` do not explain what happened, whether retrieval was attempted, why it matters, or what the trader should do next.
- The visible `Load` action feels like it should gather broad context, while the implementation currently loads price context only.
- Provider configuration, information acquisition, and operator interpretation are three different concerns that are not yet clearly separated in the UX.
- The session surfaced a possible future `Context Workbench` or dedicated research workspace for gathering and interpreting technical, fundamental, catalyst, sector, and provider-status context before thesis formation.

## Observations

- A large section of the current Opportunity Workspace is structurally organized but cognitively thin, with provenance-oriented panels occupying more visual weight than operational meaning.
- The phrase `What candidate decisions are developing?` is misleading inside a single-symbol flow and does not match how traders describe their work.
- The selected instrument can become visually anonymous in the main workspace, weakening situational anchoring.
- Empty or unavailable states often report system condition without giving trader meaning, severity, or next action.
- Price context, fundamentals context, provider configuration, and thesis development are present as system capabilities but not yet joined into a coherent operator workflow.
- The current UI already proves the value of advisory boundaries, provenance, and capability separation; the missing layer is translation into trader cognition.

## Ideas

- Establish a trader-facing language layer that translates internal ontology into operational terms without weakening canonical semantics.
- Replace empty provenance-heavy opportunity sections with higher-value cognition surfaces such as:
  - opportunity narrative
  - current signals
  - missing confirmation
  - readiness
  - bull / neutral / bear scenarios
  - contradictions and invalidation
- Introduce a persistent instrument identity pattern across workspaces with ticker, company, sector, stage, posture, and relevant market context.
- Differentiate missing-data states such as:
  - not loaded
  - provider failed
  - unsupported instrument
  - credentials missing
  - retrieval not implemented
- Separate context acquisition from opportunity evaluation through a possible future `Context Workbench`.
- Add a context interpretation layer between raw provider payloads and operator-facing cognition.
- Make advisory context acquisition explicit with actions such as loading price, fundamentals, catalysts, technicals, and broader research context.

## Questions

- What operator-facing terms should replace internal lifecycle and ontology language in each workspace?
- Should instrument identity become a mandatory cross-workspace pattern rather than a local Opportunity Workspace fix?
- Which missing-context states are operationally distinct enough to deserve separate UI states now?
- Should context acquisition remain embedded inside opportunity flows, or should a dedicated research workspace own it?
- Where should the future context interpretation layer live relative to M11 AI advisory work?
- Which of the identified issues are immediate UX bugs versus doctrine, architecture, or milestone-level follow-on work?

## Concerns

- Exposing backend semantics too directly in the frontend may preserve architectural truth while still degrading trader usability.
- A context-heavy system that does not explain meaning can still leave the operator cognitively unsupported.
- If `Market Context` remains overloaded as a label for multiple data families, future provider and workspace behavior may continue to confuse operators.
- A rushed fix that only changes copy could miss the deeper need for workspace identity, interpretation, and context-acquisition boundaries.
- This material appears to contain both near-term UX corrections and larger doctrine-level discoveries; they should not be collapsed into one implementation pass.

## Possible Next Outputs

- Feedback issue cluster
- Processed synthesis note
- UX doctrine update candidate
- Trader language doctrine candidate
- Workspace identity pattern candidate
- Context interpretation layer candidate
- Context Workbench milestone concept
