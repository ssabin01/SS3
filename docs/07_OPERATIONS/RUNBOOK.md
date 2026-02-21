# Runbook

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This runbook defines operational procedures for MVP (local-first).

---

## 1. Startup Checklist

- Verify provider credentials are present in secure storage.
- Verify snapshot store path is writable and encrypted if configured.
- Verify engine_version and conventions_version are available.

---

## 2. Common Issues

### 2.1 Provider Authentication Failures
Symptoms:
- snapshot creation fails with `AUTH_FAILED`

Actions:
- confirm token validity in secure storage
- re-authenticate via approved workflow
- confirm network access to provider endpoint

### 2.2 Missing Data in Snapshot
Symptoms:
- evaluation fails with `DATA_MISSING`

Actions:
- inspect snapshot quality summary
- re-create snapshot
- verify provider returned complete chain

### 2.3 IV Solver Non-Convergence
Symptoms:
- IV field marked unavailable with `SOLVER_NO_CONVERGENCE`

Actions:
- confirm target price is within model bounds
- confirm bid/ask midpoint logic
- review solver config and tolerances (requires ADR to change)

---

## 3. Evidence Bundle Export Procedure

- Ensure snapshot is pinned (to avoid accidental live drift).
- Export evidence bundle.
- Verify bundle includes output_hash and all required IDs.
- Store bundle under governance policy.

---

## 4. Data Retention / Purge

- Run retention enforcement per configured policy.
- Purge must securely delete:
  - snapshots and raw payloads (if allowed)
  - derived outputs
  - logs (subject to governance)
