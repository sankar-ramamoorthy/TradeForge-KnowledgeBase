---
title: ReplaySession
type: entity
status: canonical
tags: [TradeForge, entity, replay]
created: 2026-05-08
updated: 2026-05-10
aliases: [Replay Session]
---

# ReplaySession

## Definition

A ReplaySession is a reconstruction of historical system context from the [[Event Ledger]], deterministic rules, and optional historical snapshots.

## Semantic Role

ReplaySession supports auditability, decision reconstruction, behavioral review, and long-term learning.

## Authority Boundary

ReplaySession is not a live dashboard, current projection, or AI-generated summary. It must not depend on live APIs or mutable external state.

## Lifecycle Relationship

ReplaySession reconstructs lifecycle state across Idea, Thesis, Plan, Approval, Execution, Position, and Review.

## Event Relationships

ReplaySession consumes events from persona, workspace, market, scenario, decision, execution, review, and system domains.

## Replay And Review Relevance

ReplaySession is the primary object used to inspect what was known, visible, approved, executed, and reviewed at a point in time.

A ReplaySession may include a [[ReplayTimeline]] as one derived view over replay-relevant event history.

## Related Concepts

- [[Event Ledger]]
- [[ReplayTimeline]]
- [[LifecycleEvent]]
- [[Workspace]]
- [[ReviewArtifact]]
