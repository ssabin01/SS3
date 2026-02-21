# Reproducibility and Evidence Bundles

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the reproducibility contract and evidence bundle requirements.

---

## 1. Reproducibility Contract

Given an evidence bundle `B` and the referenced engine version, a third party MUST be able to:

- reconstruct the snapshot inputs
- reconstruct the trade and scenario definitions
- re-run evaluation
- reproduce the output hash

If bitwise equality is not achievable due to platform-level floating behavior, reproducing the **quantized output hash** is the compliance standard.

---

## 2. Evidence Bundle Contents (Minimum)

An evidence bundle MUST include:

### 2.1 Inputs
- snapshot:
  - snapshot_id
  - normalized instruments
  - normalized quotes
  - financing inputs (rates/dividends/borrow)
  - provenance metadata
- trade definition (canonical JSON) + trade_id
- scenario definition (canonical JSON) + scenario_id

### 2.2 Versions
- engine_version
- conventions_version
- model_version
- normalization_version

### 2.3 Outputs
- outputs (quantized)
- output_hash

### 2.4 Audit
- export timestamp
- export event id (if used)
- optional operator/host metadata (redacted as needed)

---

## 3. Export Modes

### 3.1 Internal-Only Bundles
May include encrypted raw provider payloads and full snapshot contents.

### 3.2 Redacted Bundles
Must exclude provider payloads not redistributable, but MUST still include normalized inputs sufficient to reproduce outputs.

Export mode must be declared in bundle metadata.

---

## 4. Replay Procedure (Normative)

1. Validate bundle schema and hashes of contained records.
2. Verify engine/conventions/model versions are available.
3. Load normalized snapshot inputs and compute snapshot_id; MUST match bundle snapshot_id.
4. Load trade/scenario and compute their IDs; MUST match.
5. Evaluate and compute output_hash; MUST match.

Any mismatch must be reported with a structured diff and failure codes.
