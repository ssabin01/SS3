# Architecture Overview

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


## 1. Architectural Goals

- Deterministic analytics from immutable snapshots
- Clear separation between:
  - provider ingestion
  - snapshot persistence
  - analytics evaluation
  - UI rendering
- Reproducible evidence bundles and audit traceability
- Local-first MVP with an explicit path to a future server deployment

---

## 2. High-Level Components

### UI Workstation
Responsible for:
- chain visualization
- trade building
- scenario controls
- rendering analytics outputs
- initiating exports

### Provider Adapter
Responsible for:
- obtaining market data from a configured provider API
- producing raw payload records
- translating to normalized internal schema

### Snapshot Store
Responsible for:
- persisting immutable snapshots
- storing raw payloads + normalized quotes + provenance
- providing snapshot retrieval by `snapshot_id`

### Analytics Engine
Responsible for:
- computing prices/greeks/IV for evaluation requests
- implementing model versions and conventions versions
- producing deterministic output records and hashes

### Audit Logger
Responsible for:
- recording key events for traceability
- ensuring audit events are immutable and queryable

---

## 3. Data Flow (Snapshot → Evaluation → Export)

1. Provider adapter requests market data and writes:
   - raw payload record
   - normalized snapshot entities
2. Snapshot store computes `snapshot_id` and persists the snapshot immutably.
3. UI selects a snapshot and constructs trade + scenario definitions.
4. UI sends an evaluation request to the analytics engine with:
   - snapshot_id, trade_id, scenario_id, engine/model/conventions versions
5. Analytics engine returns deterministic outputs and output hashes.
6. Export produces an evidence bundle including all required artifacts.

---

## 4. Architectural Invariants

- Analytics MUST NOT read live provider data directly.
- Analytics MUST NOT mutate snapshot data.
- Any stateful caching MUST be keyed by deterministic IDs and version fields.

---

## 5. Future Server Mode (Non-Binding)

A future deployment may split components across processes/machines, but MUST preserve the same interface contracts and determinism guarantees.

Scaling assumptions are documented in `docs/04_ARCHITECTURE/SCALING_MODEL.md`.
