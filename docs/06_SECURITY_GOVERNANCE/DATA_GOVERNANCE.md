# Data Governance

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines governance requirements for data retention, provenance, and export controls.

---

## 1. Data Classes

At minimum, classify stored data into:

- **Class A — Secrets**: tokens, credentials, encryption keys
- **Class B — Proprietary Analytics Inputs**: snapshots, scenarios, trade definitions
- **Class C — Derived Outputs**: evaluation results, greeks, IVs
- **Class D — Operational Metadata**: logs, audit events, performance traces

---

## 2. Retention

Retention policies MUST be configurable and MUST apply to:

- snapshots and raw payloads
- evaluations
- evidence bundles
- logs and audit events

Policies MUST include:
- default retention duration per class
- secure deletion behavior
- export retention vs local retention differences

---

## 3. Lineage and Provenance

Every evaluation MUST be traceable to:

- snapshot_id and snapshot provenance
- trade_id and canonical trade definition
- scenario_id and canonical scenario definition
- engine/conventions/model versions

---

## 4. Export Governance

Exports MUST support:

- internal-only bundles (may include encrypted raw payloads)
- redacted bundles (normalized-only)

Exports SHOULD be policy-gated (e.g., disable redistributable exports in restricted environments).

---

## 5. Access Control (Future Server Mode)

If server mode is introduced, entitlements and tenant isolation MUST be specified via ADR and reflected in this document.
