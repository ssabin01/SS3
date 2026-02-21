# User Flows

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document describes user-facing flows and the artifacts created in each flow. It is derived from `docs/02_REQUIREMENTS/REQUIREMENTS.md`.

---

## Flow 1 — Create or Select a Snapshot

**Goal:** Establish the immutable basis for all analysis.

1. User selects underlying(s) and a provider configuration.
2. System requests provider data and creates `snapshot_id`.
3. System persists raw inputs, normalized instruments, and provenance.
4. UI displays:
   - `snapshot_id`
   - “as-of” timestamp
   - provider metadata

Artifacts:
- `snapshot_id`
- persisted snapshot record

---

## Flow 2 — Browse a Chain and Build a Trade

1. User selects underlying within the active snapshot.
2. UI loads chain from snapshot-normalized data.
3. User selects contracts and assigns quantities.
4. System produces a deterministic `trade_id`.

Artifacts:
- `trade_id` (deterministic from trade definition)
- trade definition record

---

## Flow 3 — Define a Scenario and Evaluate

1. User defines scenario parameters (spot/vol/time/rates/dividends/borrow).
2. System produces `scenario_id`.
3. User requests evaluation for `{snapshot_id, trade_id, scenario_id}`.
4. Analytics engine returns:
   - per-leg outputs
   - aggregated trade outputs
   - output hashes

Artifacts:
- `scenario_id`
- evaluation result set, tied to engine/conventions/model versions

---

## Flow 4 — Compare Two Trades

1. User selects Trade A and Trade B.
2. System evaluates both under the same snapshot+scenario basis.
3. UI renders comparison:
   - structural diff
   - exposure diff
   - P/L diff

Artifacts:
- comparison view (derived)
- optionally persisted comparison record (implementation decision)

---

## Flow 5 — Export Evidence Bundle

1. User selects an evaluation (or a comparison view).
2. System creates an evidence bundle containing:
   - snapshot content or reference
   - trade + scenario definitions
   - outputs + hashes
   - engine/conventions/model versions
   - audit metadata
3. System logs an export audit event.

Artifacts:
- `evidence_bundle_id` (optional) and export payload
