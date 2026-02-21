# Interface Contracts

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the interface contracts between UI and local services/engine.

These contracts are technology-agnostic: they can be implemented via in-process calls, IPC, or HTTP, but the payload schemas MUST match.

---

## 1. Versioning

- Contract version prefix: `/v1`
- Backwards-incompatible changes require:
  - `/v2` etc
  - an ADR
  - updated validation coverage

---

## 2. Common Types

### 2.1 Identifiers

All IDs are strings and MUST be deterministic hashes:

- `snapshot_id`
- `trade_id`
- `scenario_id`
- `engine_version`
- `conventions_version`
- `model_version`

### 2.2 Error Envelope

All endpoints MUST return errors in this form:

```json
{
  "status": "ERROR",
  "error_code": "DATA_MISSING",
  "message": "Underlying spot missing in snapshot",
  "context": {
    "snapshot_id": "…",
    "trade_id": "…",
    "scenario_id": "…"
  }
}
```

Error codes are defined in `docs/02_REQUIREMENTS/FAILURE_POLICY.md`.

---

## 3. Snapshot API

### 3.1 Create Snapshot

`POST /v1/snapshots`

Request:

```json
{
  "provider_id": "schwab",
  "provider_config": { "redacted": true },
  "universe": {
    "underlyings": ["SPY"]
  }
}
```

Response:

```json
{
  "status": "OK",
  "snapshot_id": "sha256:…",
  "asof_ts_utc": "2026-02-19T21:15:03Z",
  "provenance": {
    "provider_id": "schwab",
    "ingest_ts_utc": "2026-02-19T21:15:05Z",
    "normalization_version": "1"
  }
}
```

### 3.2 Get Snapshot Metadata

`GET /v1/snapshots/{snapshot_id}`

Response includes:
- asof timestamp
- universe
- provenance
- quality flags summary

---

## 4. Trade API

### 4.1 Create Trade Definition

`POST /v1/trades`

Request:

```json
{
  "trade": {
    "legs": [
      {
        "instrument_id": "…",
        "qty": 1
      }
    ]
  }
}
```

Response:

```json
{
  "status": "OK",
  "trade_id": "sha256:…"
}
```

---

## 5. Scenario API

### 5.1 Create Scenario

`POST /v1/scenarios`

Request:

```json
{
  "scenario": {
    "spot_shift": { "type": "pct", "value": 0.01 },
    "vol_shift": { "type": "abs", "value": 0.02 }
  }
}
```

Response:

```json
{
  "status": "OK",
  "scenario_id": "sha256:…"
}
```

---

## 6. Evaluation API

### 6.1 Evaluate Trade

`POST /v1/evaluate`

Request:

```json
{
  "snapshot_id": "sha256:…",
  "trade_id": "sha256:…",
  "scenario_id": "sha256:…",
  "engine_version": "engine:1.0.0",
  "conventions_version": "conv:1",
  "model_version": "bs:1"
}
```

Response:

```json
{
  "status": "OK",
  "evaluation": {
    "snapshot_id": "sha256:…",
    "trade_id": "sha256:…",
    "scenario_id": "sha256:…",
    "engine_version": "engine:1.0.0",
    "conventions_version": "conv:1",
    "model_version": "bs:1",
    "outputs": {
      "trade": {
        "price": 123.45,
        "delta": 12.3
      },
      "legs": [
        {
          "instrument_id": "…",
          "price": 1.23,
          "delta": 0.45
        }
      ]
    },
    "output_hash": "sha256:…"
  }
}
```

---

## 7. Export API

`POST /v1/exports/evidence-bundle`

Request:
- references an evaluation (or provides the full input tuple)

Response:
- returns a bundle identifier and a binary or JSON payload depending on export mode

Evidence requirements are defined in `docs/05_VALIDATION/REPRODUCIBILITY.md`.
