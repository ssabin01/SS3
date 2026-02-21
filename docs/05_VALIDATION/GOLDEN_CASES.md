# Golden Cases

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the format and coverage requirements for golden case datasets.

Golden cases are the canonical mechanism for regression detection.

---

## 1. Dataset Format

A golden dataset is a directory containing:

- `inputs.json` — canonical inputs
- `expected_outputs.json` — expected quantized outputs
- `metadata.json` — versions and provenance

### 1.1 inputs.json

Must include:

- snapshot-equivalent inputs (spot, instruments, financing, asof time)
- scenario definition
- trade definition
- engine/conventions/model versions

### 1.2 expected_outputs.json

Must include:

- quantized outputs for:
  - per-leg price/greeks
  - aggregated trade outputs
- output hash

### 1.3 metadata.json

Must include:

- dataset_id
- creation date
- author
- rationale
- coverage tags

---

## 2. Coverage Requirements

Golden cases MUST cover:

- deep ITM, ATM, deep OTM
- short-dated and long-dated expiries
- low and high volatility regimes
- both calls and puts
- multi-leg aggregation (≥ 4 legs) with mixed signs
- edge cases near expiry (but with valid T > 0)

If American options are supported, separate cases must exist per model_version.

---

## 3. Versioning and Drift

- Any change to expected outputs requires:
  - an ADR (if due to convention/model change), or
  - a dataset version bump with justification

No silent updates.
