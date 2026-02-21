# Numerical Conventions

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines numerical conventions needed to guarantee determinism and reproducibility.

---

## 1. Floating-Point Standard

- All core analytics MUST use IEEE-754 binary64 (“float64”) arithmetic.
- Implementations MUST avoid non-deterministic math optimizations (e.g., fast-math) unless explicitly allowed by ADR.

---

## 2. Time and Day Count

### 2.1 Timestamp Standard

- All timestamps MUST be stored as UTC instants.
- Canonical string form: RFC 3339 / ISO-8601 with `Z` suffix (e.g., `2026-02-19T21:15:03Z`).

### 2.2 Year Fraction

MVP uses **ACT/365F**:

`year_fraction(t0, t1) = max(0, (t1 - t0).total_seconds) / (365 * 86400)`

Any change requires an ADR and a new `conventions_version`.

---

## 3. Rate Conventions

Rates are expressed as **continuously compounded** annual rates unless explicitly overridden by ADR.

- `r` = risk-free rate (annual, absolute)
- `q` = dividend yield or equivalent (annual, absolute)

If discrete dividends are supported, their representation and conversion rules MUST be specified in `docs/03_DOMAIN/RATES_DIVIDENDS_BORROW.md`.

---

## 4. Canonical Serialization

### 4.1 Canonical JSON

For hashing and evidence bundles, canonical JSON MUST:

- use UTF-8
- sort object keys lexicographically
- represent numbers as decimal strings with a defined formatting rule (see below)
- represent timestamps in the canonical format above
- avoid NaN/Inf (not valid JSON numbers)

### 4.2 Float Formatting Rule

To avoid language/runtime differences:

- Any float64 value serialized for hashing MUST be quantized (rounded) before serialization.
- Quantization grids are defined in `docs/05_VALIDATION/TOLERANCES.md` and are part of `conventions_version`.

Example rule:
- price values rounded to `PRICE_QUANTUM`
- greek values rounded to `GREEK_QUANTUM`
- IV values rounded to `IV_QUANTUM`

---

## 5. Deterministic Hashing

Deterministic IDs MUST be computed as:

`id = SHA-256(canonical_json_bytes)`

where the canonical JSON is defined above.

IDs include at minimum:

- `snapshot_id`
- `trade_id`
- `scenario_id`
- `evaluation_id` (optional)
- `evidence_bundle_id` (optional)

---

## 6. Implied Volatility Solver

IV solving MUST be deterministic.

### 6.1 Inputs
- target price (per contract unit)
- model inputs: `S, K, T, r, q`
- option type (call/put)

### 6.2 Constraints
- sigma bounds: `[SIGMA_MIN, SIGMA_MAX]` where:
  - `SIGMA_MIN = 1e-6`
  - `SIGMA_MAX = 10.0`
- max iterations: `MAX_ITERS = 100`

### 6.3 Method
The solver MUST be bracketed (e.g., bisection or hybrid with Newton inside bracket) to guarantee deterministic convergence behavior.

### 6.4 Failure
If no convergence within constraints, return `SOLVER_NO_CONVERGENCE` per failure policy.

---

## 7. Deterministic Output Hashing

When exporting evidence bundles, the system MUST provide:

- output values (quantized)
- a hash of the output set (canonical JSON → SHA-256)

This allows audit systems to detect drift even when re-evaluations are performed.
