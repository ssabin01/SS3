# Documentation Policy

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the repository-wide documentation rules so the spec stays coherent as the product evolves.

---

## Rules

1. **Single entry point**
   - `docs/INDEX.md` is the only official doc map.
   - No other file should attempt to maintain a competing index.

2. **No duplicate sources of truth**
   - Requirements are canonical in `docs/02_REQUIREMENTS/REQUIREMENTS.md`.
   - Do not restate requirements elsewhere; reference them by ID.

3. **Every normative claim has a home**
   - If you need a new binding rule, add it to the correct canonical document or write an ADR.

4. **Conflicts are resolved deterministically**
   - Precedence order is defined in `docs/INDEX.md`.

5. **Naming conventions**
   - Use `UPPER_SNAKE_CASE.md` for product/spec/policy docs.
   - Use `ADR-####-kebab-case.md` for ADRs.

6. **Change control**
   - Any change that affects determinism, numerics, snapshot semantics, security posture, or interface contracts MUST be recorded in an ADR.

---

## Document Status Levels

- **Normative**: binding requirements/specifications
- **Informative**: contextual explanation; never binding
- **Living**: state tracking; may be incomplete; never overrides normative docs

---

## Requirements Referencing

Requirements must be referenced as:

- `FR-###` (functional requirement)
- `NFR-###` (non-functional requirement)

Example: “See `FR-012` for trade-definition canonicalization.”

---

## Interface Contract Versioning

Any change to a request/response schema or field meaning requires:

- bumping the interface version
- adding an ADR
- updating validation cases
