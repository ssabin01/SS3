# Analytics Specification

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the analytics produced by the system, including conventions, outputs, and limitations.

---

## 1. Supported Instruments (MVP)

- Vanilla equity/ETF options (calls and puts)
- Underlying: equity/ETF spot price `S > 0`
- Option contract attributes:
  - strike `K > 0`
  - expiry timestamp `expiry_ts_utc`
  - option type: CALL or PUT
  - exercise style: EUROPEAN or AMERICAN (see limitations)

Instrument normalization requirements are defined in `docs/04_ARCHITECTURE/DATA_MODEL.md`.

---

## 2. Core Inputs

Analytics are computed for a tuple:

`(snapshot, scenario, trade, engine_version, conventions_version, model_version)`

### 2.1 Snapshot Inputs (Base)

- `asof_ts_utc` (timestamp of snapshot capture)
- Underlying spot `S`
- Option mid/mark quotes (for display and IV solving)
- Interest rate inputs (see `docs/03_DOMAIN/RATES_DIVIDENDS_BORROW.md`)
- Dividend and borrow inputs (see same)

### 2.2 Scenario Inputs (Overrides)

A scenario MAY override or shift:

- spot: `S'`
- volatility: `sigma'`
- time: `asof_ts_utc'` (time shift)
- rates/dividends/borrow inputs

Scenario definition is canonical in `docs/02_REQUIREMENTS/REQUIREMENTS.md` (FR-030).

---

## 3. Time-to-Expiry Convention

Time-to-expiry `T` MUST be computed from timestamps:

`T = year_fraction(asof_ts_utc', expiry_ts_utc)`

The canonical year fraction function is defined in `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md`.

If `expiry_ts_utc <= asof_ts_utc'`, then `T = 0` and analytics MUST follow the failure policy rules (e.g., intrinsic-only mode MAY be defined by ADR; otherwise this is a DOMAIN_ERROR).

---

## 4. Pricing Models (MVP)

### 4.1 Baseline Model: European Black–Scholes

For EUROPEAN options, MVP MUST implement Black–Scholes pricing under the configured conventions:

Inputs:
- `S, K, T, r, q, sigma`

Outputs:
- price
- greeks (delta, gamma, vega, theta, rho)

### 4.2 American Options (MVP)

American option pricing is a **separate, explicit model choice**.

- If American pricing is supported in MVP, the model and method MUST be defined in an ADR and referenced by `model_version`.
- If American pricing is not supported in MVP, the system MUST surface this explicitly as `ERROR` for AMERICAN instruments when model outputs are requested.

### 4.3 External Parity Mode

Parity against an external tool (e.g., Thinkorswim) is defined as a distinct mode:

- `TOS_PARITY_MODE` is specified in `docs/03_DOMAIN/TOS_PARITY_SPEC.md`.

---

## 5. Output Definitions and Units

All outputs MUST be computed deterministically per `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md`.

### 5.1 Price

**Option price** is expressed in currency units per contract unit (pre-multiplier). The contract multiplier MUST be applied when aggregating to trade value.

### 5.2 Greeks

Definitions (per contract unit, pre-multiplier):

- **Delta**: `dPrice/dS`
- **Gamma**: `d²Price/dS²`
- **Vega**: `dPrice/dSigma` where sigma is absolute (1.00 = 100% vol)
- **Theta**: `dPrice/dt` where `t` is in **years** (UI may also display per-day; conversion MUST be explicit)
- **Rho**: `dPrice/dr` where `r` is absolute (1.00 = 100% rate)

### 5.3 Implied Volatility (IV)

IV is defined as the sigma that solves:

`model_price(S, K, T, r, q, sigma) = target_price`

IV solving MUST follow solver constraints and failure rules defined in:
- `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md`
- `docs/02_REQUIREMENTS/FAILURE_POLICY.md`

---

## 6. Aggregation Rules (Trade-Level)

Given trade legs `i` with quantity `qty_i` and contract multiplier `mult_i`:

- Trade price = `Σ qty_i * mult_i * price_i`
- Trade greeks similarly aggregate as sums under the same scaling.

Any additional aggregation (e.g., margin, exposure buckets) MUST be defined in requirements and/or ADR.

---

## 7. Limitations and Explicit Non-Guarantees

- The system does not claim “true market” pricing; it provides model-based analytics under explicit assumptions.
- Parity mode is defined by tolerance envelopes; exact bitwise equivalence with third-party tools is not assumed.
