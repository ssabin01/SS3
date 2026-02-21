# Data Model

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the logical data model and required fields for persistence.

Implementations may choose SQL/SQLite/embedded stores, but the entity model MUST be preserved.

---

## 1. Entity Overview

### Snapshot
- `snapshot_id` (PK, deterministic)
- `asof_ts_utc`
- `ingest_ts_utc`
- `provider_id`
- `provider_request` (canonical JSON)
- `normalization_version`
- `quality_summary`
- `license_tags`
- `raw_payload_ref` (pointer to encrypted blob or storage key)

### Instrument
- `instrument_id` (PK, deterministic)
- `instrument_type` (UNDERLYING | OPTION)
- `underlying_id` (nullable)
- For options:
  - `expiry_ts_utc`
  - `strike`
  - `option_type` (CALL | PUT)
  - `exercise_style` (EUROPEAN | AMERICAN | UNKNOWN)
  - `multiplier`
  - `currency`

### Quote
Quotes are always snapshot-scoped.

- `snapshot_id` (FK)
- `instrument_id` (FK)
- `bid`, `ask`, `mid` (nullable as allowed)
- `last` (optional)
- `quote_ts_utc` (provider quote timestamp)
- `quality_flag` (OK/STALE/MISSING/SUSPECT)

### TradeDefinition
- `trade_id` (PK, deterministic)
- `trade_canonical_json`
- `created_ts_utc`

### ScenarioDefinition
- `scenario_id` (PK, deterministic)
- `scenario_canonical_json`
- `created_ts_utc`

### EvaluationResult
- `evaluation_id` (PK, optional deterministic)
- `snapshot_id`
- `trade_id`
- `scenario_id`
- `engine_version`
- `conventions_version`
- `model_version`
- `outputs_canonical_json` (quantized)
- `output_hash`
- `status` (OK/WARN/ERROR)
- `error_codes` (if any)
- `created_ts_utc`

### AuditEvent
- `audit_event_id` (PK, deterministic recommended)
- `ts_utc`
- `event_type`
- `actor` (user/process)
- `context_ids` (snapshot_id/trade_id/scenario_id/evaluation_id/bundle_id)
- `details_canonical_json`

---

## 2. Required Indexes (Logical)

- Snapshot lookup by `snapshot_id`
- Quotes lookup by `(snapshot_id, instrument_id)`
- Evaluation lookup by `(snapshot_id, trade_id, scenario_id, engine_version, conventions_version, model_version)`
- Audit lookup by `ts_utc` and `event_type`

---

## 3. Storage of Raw Provider Payloads

Raw payloads MUST be stored either:

- encrypted at rest, keyed by a reference in the snapshot record, or
- in a structured form that is provably lossless

The choice must satisfy security and licensing constraints.
