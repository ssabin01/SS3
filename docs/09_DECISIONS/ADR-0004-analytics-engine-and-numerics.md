# ADR-0004: Analytics Engine and Numerical Determinism

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


**Decision status:** Accepted  
**Date:** 2026-02-19

## Context

Options analytics require numerically stable computation. Different languages and compilers can produce small floating-point differences. Institutional determinism requires a defined approach.

## Decision

1. Core analytics uses IEEE-754 float64.
2. Outputs are quantized on a defined grid prior to hashing and validation.
3. IV solving uses a deterministic bracketed method with fixed bounds and iteration limits.
4. Any change to day count, solver configuration, or quantization requires a new `conventions_version`.

## Consequences

- Deterministic output hashes become achievable across platforms within a defined tolerance envelope.
- Validation and parity tests can detect meaningful drift.

## Alternatives Considered

- Bitwise determinism without quantization: rejected due to cross-platform float variance risk.
- Decimal arithmetic everywhere: rejected due to performance and implementation complexity for MVP.
