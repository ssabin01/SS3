# Requirements

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


## 0. Purpose

This document is the **canonical requirements specification** for AegisRisk.

Other documents may explain, elaborate, or provide examples, but **binding product behavior** is defined here.

---

## 1. System Invariants (Must Hold)

### INV-001 Snapshot-first analytics
All analytic outputs MUST be computed as a pure function of:

- `snapshot_id` (and snapshot content)
- `scenario_definition` (or `scenario_id`)
- `trade_definition` (or `trade_id`)
- `engine_version`
- `conventions_version` (day-count, compounding, rounding rules)
- `model_version` (pricing model choice, if applicable)

### INV-002 Deterministic identity
The system MUST compute deterministic IDs for snapshot/trade/scenario and MUST expose them in UI and export artifacts. See `docs/04_ARCHITECTURE/STATE_AND_IDENTITY.md`.

### INV-003 No silent fallback
Any missing/invalid inputs MUST produce explicit warnings/errors per `docs/02_REQUIREMENTS/FAILURE_POLICY.md`.

### INV-004 Evidence export
For any displayed analytics, the system MUST be able to export an evidence bundle that supports replay. See `docs/05_VALIDATION/REPRODUCIBILITY.md`.

---

## 2. Functional Requirements

### Market Data & Snapshots

**FR-001 Create snapshots**  
The system MUST be able to create an immutable snapshot from a configured market data provider adapter.

**FR-002 Persist raw + normalized inputs**  
Each snapshot MUST store:
- raw provider payloads (or a lossless representation)
- normalized quotes and instrument definitions used for analytics
- provenance metadata (provider, request parameters, timestamps)

**FR-003 Snapshot immutability**  
Snapshots MUST be immutable once created. Any refresh MUST create a new snapshot.

**FR-004 Pinned analysis**  
When a user selects a pinned snapshot, all subsequent analytics MUST reference that pinned snapshot until the user changes it.

**FR-005 Snapshot display**  
UI MUST display snapshot identifiers and the “as-of” timestamp used for the current view.

### Instruments & Chains

**FR-010 Instrument normalization**  
The system MUST normalize instruments into an internal canonical representation with stable identifiers.

**FR-011 Chain retrieval**  
The system MUST retrieve and display an options chain for a selected underlying (within snapshot scope).

**FR-012 Chain filtering/sorting**  
The chain view MUST support filtering and sorting at minimum by:
- expiry
- strike
- call/put
- moneyness / delta (if delta available)

### Trade Builder

**FR-020 Create a trade from chain selections**  
A user MUST be able to construct a trade by selecting one or more chain contracts and specifying quantities.

**FR-021 Multi-leg support**  
Trades MUST support multiple legs with independent quantities and option types.

**FR-022 Deterministic trade definition**  
The same set of legs, quantities, and parameters MUST produce the same `trade_id`.

**FR-023 Trade editing**  
Users MUST be able to:
- change quantities
- remove legs
- clone a trade definition

### Scenarios

**FR-030 Scenario definition**  
A scenario MUST be definable as explicit overrides and/or transformations applied to a base snapshot:
- spot shift (absolute or percentage)
- volatility shift (absolute or relative)
- time shift (calendar or year-fraction)
- rates override
- dividends/borrow override

**FR-031 Scenario determinism**  
A scenario definition MUST produce a deterministic `scenario_id`.

**FR-032 Scenario application**  
Applying a scenario MUST produce an evaluation result set referencing snapshot/trade/scenario IDs.

### Analytics Outputs

**FR-040 Price and IV**  
The system MUST provide, per leg and aggregated trade level:
- model price
- implied volatility (when solvable)

**FR-041 Greeks**  
The system MUST provide, per leg and aggregated trade level, greeks at minimum:
- delta
- gamma
- vega
- theta
- rho  
Units are defined in `docs/03_DOMAIN/ANALYTICS_SPEC.md`.

**FR-042 Aggregation rules**  
Aggregation MUST be deterministic and explicitly defined (sum of leg values with contract multipliers and quantities).

**FR-043 Sensitivity under scenario**  
The system MUST compute and display the delta between:
- baseline evaluation
- scenario evaluation

### Trade Comparison

**FR-050 Compare trades**  
The system MUST compare two trade definitions under the same snapshot+scenario basis and show:
- structural difference (legs/quantities)
- price and greek differences
- P/L profile differences (where defined)

### Export & Audit

**FR-060 Export evidence bundle**  
The system MUST export an evidence bundle containing:
- snapshot content (or snapshot reference + required retrieval artifacts)
- trade definition
- scenario definition
- engine/conventions/model versions
- outputs and output hashes
- validation metadata (if available)

**FR-061 Audit events**  
Key events MUST be logged:
- snapshot creation
- scenario creation
- trade creation/modification
- evaluation execution
- export events  
Audit schema is defined in `docs/06_SECURITY_GOVERNANCE/AUDIT_LOGGING.md`.

---

## 3. Non-Functional Requirements

**NFR-001 Determinism across runs**  
Given identical inputs, outputs MUST be bitwise identical where feasible; where not feasible, outputs MUST be within tolerance envelopes and differences MUST be explainable by documented non-determinism sources.

**NFR-002 Latency budgets**  
Interactive operations MUST meet budgets defined in `docs/02_REQUIREMENTS/PERFORMANCE_BUDGETS.md`.

**NFR-003 Reproducibility**  
A third party MUST be able to reproduce an exported result set using only the evidence bundle and the specified engine version.

**NFR-004 Security posture**  
Secrets, tokens, and sensitive data handling MUST comply with `docs/06_SECURITY_GOVERNANCE/SECURITY_MODEL.md`.

**NFR-005 Offline tolerance**  
If the provider is unavailable, pinned snapshot evaluation MUST remain available from persisted snapshot data.

---

## 4. Requirement Traceability

- Validation mapping: `docs/05_VALIDATION/VALIDATION_STRATEGY.md`
- Interface mapping: `docs/04_ARCHITECTURE/INTERFACE_CONTRACTS.md`
