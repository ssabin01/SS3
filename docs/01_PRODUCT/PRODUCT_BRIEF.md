# Product Brief

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


## Product Name

**AegisRisk** (working name)

## Target Users

- Hedge fund options traders
- Portfolio managers (PMs)
- Risk managers
- Quant/strat support (secondary)

## Core Job-to-be-Done

Given current market data, a proposed options trade, and a scenario, provide:

- deterministic analytics (price/greeks/IV)
- trade comparison (structure, exposures, P/L)
- reproducible evidence (snapshot + scenario + config) that can be replayed

## Product Constraints (Binding)

- The system MUST be deterministic as defined in:
  - `docs/09_DECISIONS/ADR-0002-determinism-and-snapshots.md`
  - `docs/04_ARCHITECTURE/STATE_AND_IDENTITY.md`
- Analytics MUST be computed from an immutable **snapshot** (no hidden live recompute).
- All externally-sourced inputs MUST be persisted with provenance metadata.

## MVP Capabilities

- Market snapshot creation from a provider adapter (see `docs/03_DOMAIN/DATA_SOURCES.md`)
- Options chain view and filtering
- Trade builder (multi-leg)
- Scenario definition and application
- Analytics and trade comparison views
- Exportable reproducibility bundle

## Explicit Non-Goals (MVP)

- Order execution or routing
- Portfolio accounting / official books & records
- Real-time streaming at exchange-native tick rates
- Cross-user shared state / multi-tenant deployment (future)

## Success Criteria

- A third party can reproduce any displayed analytic output given the exported evidence bundle.
- Deterministic hashes/IDs uniquely identify:
  - the snapshot
  - the trade definition
  - the scenario
  - the engine version
  - the output set
