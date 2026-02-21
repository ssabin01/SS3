# Data Sources and Provenance

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the minimum provenance metadata required for institutional auditability.

---

## 1. Data Source Categories

- Market data provider(s): underlying spot, option chains, quotes, instrument metadata
- Reference rates/dividend sources (if separate)
- User-provided overrides (scenarios)

---

## 2. Provenance Requirements (Snapshot)

Each snapshot MUST store:

- `provider_id` (string)
- `provider_environment` (e.g., paper/live, if applicable)
- `provider_request` (canonical representation of request parameters)
- `provider_response_raw` (lossless or encrypted lossless form)
- `ingest_ts_utc` (time of ingestion)
- `asof_ts_utc` (time the snapshot represents; may equal ingest time)
- `normalization_version`
- `license_tags` (if required by contract)

---

## 3. Data Quality Flags

Snapshots and quotes MUST support quality flags, at minimum:

- `OK`
- `STALE`
- `MISSING`
- `SUSPECT`

Quality flags MUST propagate into evaluation outputs as WARN/ERROR per failure policy.

---

## 4. Licensing and Redistribution

Evidence bundles and exports MUST include a mechanism to:

- redact or omit raw provider payloads if redistribution is prohibited
- still preserve enough information for internal replay (e.g., via encrypted payloads)

The export format MUST declare whether it is:
- `INTERNAL_ONLY`
- `REDISTRIBUTABLE`

---

## 5. PII / Sensitive Data

Provider authentication tokens, account identifiers, and any user-identifying information MUST NOT be stored in snapshots.

Security requirements are defined in:
- `docs/06_SECURITY_GOVERNANCE/SECURITY_MODEL.md`
