# Rates, Dividends, and Borrow Policy

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines how the system sources and applies rates, dividends, and borrow assumptions.

These inputs materially affect pricing and greeks and therefore MUST be explicit and persisted.

---

## 1. Required Fields in a Snapshot

Each snapshot MUST include a `financing` object containing:

- `risk_free_curve` (or a flat proxy)
- `dividend_model` (continuous yield or discrete schedule)
- `borrow_model` (if applicable)

If the provider does not supply these, the snapshot MUST explicitly record:

- the fallback method used
- the numeric values used
- the reason code (e.g., `PROVIDER_MISSING_FIELD`)

---

## 2. Risk-Free Rate

### 2.1 Default (MVP)

MVP supports at minimum:

- flat risk-free rate `r_flat`

This rate MUST be stored with:

- value
- compounding convention (continuous per `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md`)
- source (provider vs user vs config)

### 2.2 Curve (Optional)

If a term structure is supported, it MUST define:

- interpolation method
- extrapolation behavior
- bumping rules for scenarios

---

## 3. Dividends

Dividends may be represented in one of two canonical forms:

### 3.1 Continuous Yield
- continuous dividend yield `q`

### 3.2 Discrete Dividends
- schedule of `(ex_date_utc, amount)`  
and a defined conversion method to a pricing-equivalent model input.

Any discrete-dividend method requires an ADR due to high model sensitivity.

---

## 4. Borrow / Funding Spread

Borrow is represented as:

- either a continuous borrow rate `b` (annual, absolute)
- or a borrow spread added to `r` in a defined way

Borrow is optional for MVP, but if omitted it MUST be explicitly recorded as `b = 0` with a “not modeled” flag.

---

## 5. Scenario Overrides

Scenarios MAY override:

- `r` and/or curve
- `q` / dividend schedule
- borrow rate

All overrides MUST be explicit; no hidden “use latest curve” behavior is allowed in deterministic mode.
