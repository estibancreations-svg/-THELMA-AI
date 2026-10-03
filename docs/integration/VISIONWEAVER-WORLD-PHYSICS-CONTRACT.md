# T.H.E.L.M.A. ↔ VisionWeaver World Physics Contract

**Status:** DESIGN CONTRACT  
**Date:** 2026-10-03

## Purpose

T.H.E.L.M.A. coordinates governed execution around VisionWeaver's world model, scene dissection, continuity, and environmental-physics workflows.

T.H.E.L.M.A. does not replace VisionWeaver's creative/directorial authority. It orchestrates approved jobs, validates inputs, routes specialized agents, records evidence, and escalates failures.

## Governed job types

- scene dissection
- world reconstruction
- off-screen source inference
- acoustic source mapping
- avatar perception/reaction analysis
- camera continuity analysis
- environmental physics analysis
- continuity capsule creation
- scene extension preparation
- post-generation verification

## Required contract fields

Each job should carry:
- production/project ID
- scene ID
- shot/segment ID
- source asset ID
- world-state version
- avatar-state version
- environment-state version
- camera-state version
- confidence/uncertainty fields
- approval state
- execution evidence
- output artifact references
- audit correlation ID

## Governance rules

- no consequential production state should be silently overwritten;
- inferred off-screen entities must preserve uncertainty;
- autonomous continuation may propose but must respect Director/Guild approval gates;
- failed or ambiguous world reconstruction should escalate rather than fabricate certainty;
- downstream systems consume versioned state, not informal prompt text;
- every state transition must be auditable.

## Orchestration sequence

**Ingest → Validate → Dissect → Reconstruct → Physics Analysis → Perception/Reaction Analysis → Camera/Continuity Analysis → Quality Check → Approval → Persist → Handoff**

## Cross-system handoff

VisionWeaver owns:
- creative world state
- character/avatar continuity
- director decisions
- scene/camera intent
- production asset truth

T.H.E.L.M.A. owns:
- job orchestration
- routing
- policy enforcement
- validation
- failure handling
- audit
- escalation
- cross-system coordination

EC Integration Fabric should provide durable authorization, queueing, retries, dead-letter handling, and state transitions.

## Deferred

Carbon/emissions tracking is recorded for future logistics integration and is out of scope for this contract.
