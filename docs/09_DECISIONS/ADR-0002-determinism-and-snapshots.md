# ADR-0002: Determinism and Snapshots

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


**Decision status:** Accepted  
**Date:** 2026-02-19

## Context

Hedge fund users require reproducibility, auditability, and deterministic analytics. Vendor tools often recompute against moving markets and implicit assumptions, making outputs hard to audit.

## Decision

1. All analytics are snapshot-based.
2. Snapshots are immutable.
3. Trade definitions and scenario definitions are content-addressed with deterministic IDs.
4. Output hashes are computed from quantized outputs to support deterministic replay.

## Consequences

- The system can export evidence bundles that support third-party reproduction.
- Live data can exist only as a sequence of immutable snapshots.

## Alternatives Considered

- Live recompute without snapshot capture: rejected due to audit failure risk.
- Determinism without output hashing: rejected due to drift detection requirements.
