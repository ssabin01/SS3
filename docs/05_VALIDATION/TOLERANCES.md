# Tolerances and Quantization

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines:

- how outputs are quantized for hashing and storage
- how numerical comparisons are evaluated for validation and parity

---

## 1. Quantization Grids (Binding)

Quantization is applied to outputs before hashing and before writing `expected_outputs.json` in golden datasets.

### Default Quantization

| Output type | Quantum |
|---|---:|
| Price | 1e-8 |
| IV | 1e-8 |
| Delta | 1e-10 |
| Gamma | 1e-12 |
| Vega | 1e-10 |
| Theta | 1e-10 |
| Rho | 1e-10 |

Quantization is part of `conventions_version`. Any change requires an ADR.

---

## 2. Comparison Rule

Given values `a` and `b`:

- absolute difference: `abs(a - b)`
- relative difference: `abs(a - b) / max(1, abs(b))`

A comparison passes if:

`abs(a - b) <= max(abs_tol, rel_tol * max(abs(a), abs(b), 1))`

---

## 3. Validation Tolerances (Internal)

For golden tests and replay tests (internal mode), use:

- `abs_tol = quantum` (per output type)
- `rel_tol = 0` (default)  
Rationale: quantization already defines the acceptable rounding grid.

---

## 4. Parity Tolerances (External)

For parity mode, tolerance envelopes may be wider and are parity-versioned.

Default parity tolerances (unless a parity ADR overrides):

- `abs_tol = 10 * quantum`
- `rel_tol = 1e-6`

Parity tolerance changes require a new parity version (see `docs/03_DOMAIN/TOS_PARITY_SPEC.md`).

---

## 5. Pass/Fail Semantics

- Any output outside tolerance is a failure for that output.
- If any core output fails (price, delta), the evaluation is considered parity-failed for that instrument/trade.
- Non-core outputs (e.g., rho) may be treated as “advisory” only if explicitly declared by ADR.
