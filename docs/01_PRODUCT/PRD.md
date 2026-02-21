# Product Requirements Document

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


## 1. Problem

Options analytics workflows inside hedge funds often suffer from:

- non-reproducible outputs (vendor tools recompute against moving data without evidence capture)
- hidden assumptions (rates/dividends/borrow conventions not recorded)
- inconsistent parity across tools (model mismatch, day count mismatch, rounding mismatch)
- poor auditability (cannot explain “what data produced this number?”)

AegisRisk solves this by making snapshots, conventions, and numerics explicit and reproducible.

## 2. Goals (MVP)

G1. Deterministic outputs from immutable snapshots  
G2. Fast trade construction and comparison from options chains  
G3. Scenario analysis with explicit input assumptions  
G4. Exportable evidence bundle for audit/replay  
G5. Parity mode against an external reference tool, bounded by explicit tolerances (optional MVP slice)

## 3. Non-Goals (MVP)

NG1. Order execution or broker routing  
NG2. Portfolio accounting / official P&L reconciliation  
NG3. A full multi-user cloud service with tenancy/entitlements  
NG4. Realtime market data at exchange tick granularity

## 4. Primary User Stories (MVP)

US-01: As a trader, I can view an options chain and quickly build a multi-leg trade.  
US-02: As a trader, I can apply scenarios (spot/vol/time/rates/dividends) and see deterministic impacts.  
US-03: As a PM, I can compare two candidate trades (or a trade vs baseline) with clear deltas.  
US-04: As a risk manager, I can export an evidence bundle that reproduces all shown outputs.  
US-05: As a user, I can see explicit warnings/errors when inputs are missing/invalid.

## 5. Product Principles (Binding)

P1. **Snapshot-first**: all analytics reference a snapshot ID, not “the market”.  
P2. **No hidden inference**: any rates/dividends/borrow assumptions must be explicit and persisted.  
P3. **Deterministic computation**: same inputs → same outputs, across runs and machines (within defined tolerances).  
P4. **Fail loud**: no silent NaNs, no silent fallbacks.  
P5. **Parity is scoped**: external parity is defined as an explicit mode with versioning and tolerance rules.

## 6. Scope Boundary (Pointers)

- MVP scope and phased plan: `docs/01_PRODUCT/SCOPE.md`
- Canonical functional requirements: `docs/02_REQUIREMENTS/REQUIREMENTS.md`

## 7. Open Questions (Track via ADRs)

Any of the following MUST be resolved via ADR before implementation finalization:

- Pricing approach for American-style options in non-parity mode
- Packaging model for MVP (desktop-only vs embedded local service vs server option)
- Persistence technology selection and encryption strategy
- Market data provider integration details and legal constraints
