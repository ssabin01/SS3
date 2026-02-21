# Schwab-Blows-2 — Deterministic Options Analytics & Trade Comparison (AegisRisk)

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This repository specifies (and ultimately implements) a **hedge-fund-facing deterministic options analytics and trade comparison system**.

The system is designed so that:

- The same inputs (market snapshot + scenario + trade definition + engine version) produce the same outputs.
- Every output can be reproduced from a persisted evidence bundle suitable for audit.

**Documentation is the product right now.** Code and tooling must conform to the specifications in `/docs`.

---

## Start Here

- Read: `docs/INDEX.md`

---

## What This System Is

AegisRisk is a deterministic analytics workstation for:

- Options chain analytics and surfaces
- Trade building and comparison
- Scenario analysis
- Reproducible snapshot-based workflows
- Parity mode against an external reference (see `docs/03_DOMAIN/TOS_PARITY_SPEC.md`)

---

## What This System Is Not

- Not an order management system (OMS)
- Not an execution management system (EMS)
- Not a broker router
- Not a portfolio accounting system

---

## Documentation Governance

Conflict resolution and document precedence are defined in:

- `docs/INDEX.md`
- `docs/09_DECISIONS/ADR-0001-documentation-governance.md`
