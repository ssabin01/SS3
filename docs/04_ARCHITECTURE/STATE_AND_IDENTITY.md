# State and Identity

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines deterministic identity for snapshots, trades, scenarios, and outputs.

---

## 1. Design Goals

- Any analyst can refer to an evaluation by its IDs and reproduce it exactly.
- IDs are content-addressed (hash of canonical content).
- IDs are stable across machines and runs.

---

## 2. Canonical Content Forms

### 2.1 Trade Canonical Form

Trade canonical JSON MUST include:

- list of legs, each with:
  - instrument_id
  - quantity (integer)
  - any per-leg parameters (if any)
- trade-level parameters (if any)

Leg ordering MUST be deterministic. Recommended rule:
- sort legs by `(instrument_id, qty, leg_params_hash)`.

### 2.2 Scenario Canonical Form

Scenario canonical JSON MUST include:

- explicit overrides/shifts
- any implicit defaults (serialized explicitly) to avoid hidden behavior

### 2.3 Evaluation Canonical Form

Evaluation canonical JSON MUST include the full input tuple:

- snapshot_id
- trade_id
- scenario_id
- engine_version
- conventions_version
- model_version

---

## 3. Hashing Standard

- Hash algorithm: SHA-256
- ID representation: `sha256:<hex>`

---

## 4. Output Hashing

### 4.1 Output Canonicalization
Before hashing:

- outputs MUST be quantized per `docs/05_VALIDATION/TOLERANCES.md`
- serialized into canonical JSON
- hashed via SHA-256

### 4.2 Output Hash Use Cases

- Detect drift between versions
- Assert deterministic replay of evidence bundles
- Provide audit evidence for clients

---

## 5. Version Fields

Every evaluation MUST carry:

- `engine_version` — implementation build/version
- `conventions_version` — day count/rounding/solver settings
- `model_version` — pricing model selection/version

Version fields are part of determinism: changing them changes the meaning of outputs and MUST change evaluation identity.
