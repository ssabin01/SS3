# Scope and Phasing

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


## Scope Definitions

### In Scope (MVP)

S-01. Snapshot creation from a single market data provider adapter  
S-02. Options chain visualization and filtering  
S-03. Trade builder with multi-leg strategies  
S-04. Scenario definition (spot/vol/time/rates/dividends/borrow)  
S-05. Deterministic analytics outputs (price/greeks/IV) from snapshots  
S-06. Trade comparison views (structure + exposures + P/L deltas)  
S-07. Exportable reproducibility bundle (snapshot + scenario + trade + engine version + results)

### Out of Scope (MVP)

OOS-01. Order placement and routing  
OOS-02. Portfolio accounting and official P&L  
OOS-03. Multi-tenant cloud hosting and entitlements  
OOS-04. High-frequency tick replay and microstructure analytics  
OOS-05. Complex exotics beyond vanilla equity/ETF options

---

## Phasing

### Phase 0 — Spec and Evidence Format

- Finalize determinism rules, identity/hashing, and snapshot semantics
- Define evidence bundle formats and validation harness

### Phase 1 — Deterministic Analytics Core

- Implement analytics engine with fixed conventions
- Produce golden case suite and CI determinism checks

### Phase 2 — Market Data Adapter + Snapshot Store

- Implement provider adapter
- Persist raw inputs + normalized snapshot + provenance

### Phase 3 — UI Workstation

- Implement chain view, trade builder, scenario controls
- Deterministic rendering and export workflows

### Phase 4 — Parity Mode (Optional MVP Slice)

- Implement parity mode against external reference tool
- Define parity versioning and tolerance envelope

---

## Scope Change Control

Any scope expansion MUST be captured via:

- an ADR (if architectural or cross-cutting), and/or
- an update to requirements and validation coverage
