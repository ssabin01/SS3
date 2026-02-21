# Failure Policy

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This policy defines how the system surfaces failures. It is canonical for error/warning behavior.

---

## 1. Principles

1. **Fail loud**: No silent NaNs, no silent fallbacks.
2. **Typed outcomes**: Every evaluation returns either:
   - `OK`
   - `WARN` (results produced but degraded, with explicit reasons)
   - `ERROR` (results not produced or unusable)
3. **Evidence-preserving**: Even on failure, the system MUST record:
   - inputs used
   - failure codes
   - engine version
   - trace identifiers (if available)
4. **Deterministic failure**: Given identical inputs, failure classification MUST be deterministic.

---

## 2. Failure Taxonomy

### 2.1 Data Failures

- `DATA_MISSING`: required quote/input not present in snapshot
- `DATA_STALE`: data older than allowed threshold (threshold defined in deployment config)
- `DATA_INCONSISTENT`: malformed or internally inconsistent provider payload
- `INSTRUMENT_UNKNOWN`: instrument cannot be normalized

### 2.2 Numerical Failures

- `SOLVER_NO_CONVERGENCE`: IV solver did not converge within bounds/iterations
- `DOMAIN_ERROR`: invalid model domain (e.g., negative time-to-expiry)
- `OVERFLOW_UNDERFLOW`: numerical overflow/underflow detected
- `PRECISION_LOSS`: catastrophic cancellation / precision loss detected (where detectable)

### 2.3 Configuration Failures

- `CONVENTION_UNSPECIFIED`: required convention is missing (rates/day count)
- `VERSION_MISMATCH`: engine/model/conventions versions inconsistent with evidence bundle

### 2.4 Security Failures

- `AUTH_REQUIRED`: provider token missing
- `AUTH_FAILED`: provider auth rejected
- `POLICY_VIOLATION`: operation violates governance rules (e.g., export disabled)

---

## 3. WARN vs ERROR Rules

### WARN (degraded but usable)
- A subset of instruments lacks non-critical fields but core pricing inputs exist
- IV unavailable but price + greeks computable under defined model path
- Data is stale but within a configured “soft” limit and user explicitly allowed it

### ERROR (results blocked)
- Missing spot price for underlying
- Missing expiry/strike/type for a leg
- Negative or zero time-to-expiry where model requires positive
- Any condition that would produce NaN/Inf in a core output without safe handling

---

## 4. UI Requirements

- UI MUST present failure codes and human-readable messages.
- UI MUST include the identifiers:
  - `snapshot_id`
  - `trade_id`
  - `scenario_id`
  - engine/conventions/model versions
- UI MUST allow exporting the failure evidence bundle (inputs + failure details).
