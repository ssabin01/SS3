# Snapshot Model

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document is canonical for snapshot semantics and lifecycle.

---

## 1. Snapshot Definition

A snapshot is an immutable capture of:

- underlying quote(s)
- option chain instruments and quotes
- rates/dividends/borrow inputs
- provenance and quality metadata

All analytics MUST reference a `snapshot_id`.

---

## 2. Snapshot Lifecycle

### 2.1 Creation
- Snapshot is created by ingesting provider data.
- Snapshot store computes `snapshot_id` deterministically from:
  - normalized snapshot content
  - normalization version
  - provider identity
  - as-of timestamp policy

### 2.2 Immutability
- Once persisted, a snapshot MUST NOT be modified.
- Any refresh creates a new snapshot.

### 2.3 Pinning
- UI may select a pinned snapshot as the analysis basis.
- While pinned, analytics MUST NOT switch to newer snapshots automatically.

---

## 3. Snapshot ID Construction

The canonical method is:

1. Create canonical JSON of:
   - normalized instruments
   - normalized quotes (quantized as needed)
   - financing inputs (rates/dividends/borrow)
   - `asof_ts_utc`
   - `provider_id`
   - `normalization_version`
2. Compute `snapshot_id = SHA-256(canonical_json_bytes)`

Canonical JSON rules are defined in `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md`.

---

## 4. Live Snapshot Policy (MVP)

If “live mode” exists:

- live mode MUST create new immutable snapshots on each refresh
- live mode MUST never mutate the currently pinned snapshot
- UI MUST surface:
  - whether analysis is pinned or live
  - the active snapshot_id

---

## 5. Snapshot Consistency Requirements

A snapshot MUST be internally consistent:

- all quotes reference known instruments
- all instruments required by a chain are included
- timestamps and provenance fields are present

Inconsistencies must be surfaced as `DATA_INCONSISTENT` failures.
