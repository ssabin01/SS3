# Audit Logging

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines audit event requirements for traceability and institutional review.

---

## 1. Audit Principles

- Audit events are **append-only**.
- Events MUST include deterministic context identifiers.
- Events MUST not contain secrets.

---

## 2. Required Event Types

Minimum event types:

- `SNAPSHOT_CREATED`
- `TRADE_CREATED`
- `TRADE_UPDATED` (if updates exist; otherwise omit and rely on immutable new trade_id)
- `SCENARIO_CREATED`
- `EVALUATION_RUN`
- `EXPORT_CREATED`
- `PROVIDER_AUTH_FAILED`
- `DATA_POLICY_VIOLATION`

---

## 3. Event Schema

Each event MUST include:

- `audit_event_id` (deterministic recommended)
- `ts_utc`
- `event_type`
- `actor` (user/process identifier)
- `context`:
  - snapshot_id (optional depending on event)
  - trade_id
  - scenario_id
  - evaluation_id
  - bundle_id
- `details` (canonical JSON)

---

## 4. Deterministic Event IDs (Recommended)

`audit_event_id = SHA-256(canonical_json(event_without_id))`

This allows detection of accidental duplication and supports replay-based auditing.

---

## 5. Storage and Queryability

Audit events MUST be queryable by:

- time range
- event type
- context IDs

If encryption at rest is used, querying requirements must still be satisfied (e.g., via encrypted fields or secure indexing).
