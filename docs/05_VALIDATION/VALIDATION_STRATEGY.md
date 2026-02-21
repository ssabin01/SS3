# Validation Strategy

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines how the system will validate correctness, determinism, and parity.

---

## 1. Validation Objectives

V1. Validate pricing and greeks under baseline model conventions  
V2. Validate determinism: replay produces identical outputs (or identical quantized outputs)  
V3. Validate failure classification and error envelopes  
V4. Validate parity mode (if enabled) against reference tool within tolerance envelope  
V5. Validate performance budgets (p95) against fixed evidence bundles

---

## 2. Test Taxonomy

### 2.1 Golden Tests (Deterministic)
- Fixed inputs → expected outputs within tolerance.
- Stored as versioned datasets (see `GOLDEN_CASES.md`).

### 2.2 Property-Based Tests (Deterministic)
- Invariants that should hold for wide input ranges:
  - monotonicity in spot for calls/puts under assumptions
  - put-call parity under specified conditions (where applicable)

### 2.3 Replay Tests (Deterministic)
- Load an evidence bundle, re-run evaluation, and compare:
  - output hashes (preferred)
  - or quantized outputs (fallback)

### 2.4 Negative/Failure Tests
- Missing inputs
- Invalid domain conditions
- Solver non-convergence cases

### 2.5 Parity Tests (Optional)
- Compare outputs to reference tool under parity version rules.

### 2.6 Performance Tests
- Run fixed evaluation workloads and assert budgets in `docs/02_REQUIREMENTS/PERFORMANCE_BUDGETS.md`.

---

## 3. Validation Gates

A build is considered valid when:

- all golden tests pass
- replay tests match output hashes
- failure handling tests match taxonomy
- performance tests meet budgets (where enforced)

---

## 4. Evidence and Traceability

Every validation run SHOULD record:

- engine_version
- conventions_version
- model_version
- dataset version
- output hashes
