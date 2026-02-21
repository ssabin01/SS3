# Performance Budgets

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines performance expectations for MVP. These are **budgets**, not guarantees; exceeding budgets is a validation failure unless an ADR updates them.

---

## 1. Assumptions

- Single-user workstation
- Local snapshot store on SSD-class storage
- Analytics evaluated on CPU
- Chain sizes typical of liquid equity/ETF underlyings (no exotic universe assumptions)

---

## 2. Latency Budgets (Interactive)

All budgets are **p95** unless otherwise specified.

### UI/Workflow Budgets

- PB-001 Load chain from persisted snapshot: **≤ 300 ms**
- PB-002 Add/remove a leg in trade builder (including recompute): **≤ 150 ms**
- PB-003 Apply scenario tweak (spot/vol/time) and recompute: **≤ 250 ms**
- PB-004 Compute a full trade evaluation (multi-leg ≤ 8 legs): **≤ 300 ms**
- PB-005 Compute trade comparison (two evaluations + diff): **≤ 600 ms**
- PB-006 Export evidence bundle: **≤ 2 s**

---

## 3. Throughput Budgets (Batch)

- PT-001 Evaluate N trades under one snapshot+scenario where N=100 and legs/trade ≤ 8: **≤ 10 s**

---

## 4. Caching Rules

Caching MAY be used, but MUST NOT violate determinism:

- Cache keys MUST include all inputs that affect outputs:
  - snapshot_id, trade_id, scenario_id, engine version, conventions version, model version
- Cache invalidation MUST be deterministic.

---

## 5. Measurement

Performance tests MUST:

- pin CPU affinity where possible
- record engine build/version metadata
- run against fixed evidence bundles to avoid market-data variability

Validation mapping: `docs/05_VALIDATION/VALIDATION_STRATEGY.md`
