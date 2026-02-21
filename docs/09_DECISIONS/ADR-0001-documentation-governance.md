# ADR-0001: Documentation Governance

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


**Decision status:** Accepted  
**Date:** 2026-02-19

## Context

The repository is documentation-heavy and pre-code. Without explicit governance, multiple documents can become competing “sources of truth,” increasing drift risk and creating ambiguous requirements.

## Decision

1. `docs/INDEX.md` is the single documentation map.
2. Precedence order is defined in `docs/INDEX.md` and is binding.
3. Requirements are canonical in `docs/02_REQUIREMENTS/REQUIREMENTS.md`.
4. Any change affecting determinism, numerics, snapshots, security, or interfaces requires an ADR.

## Consequences

- Documentation conflicts can be resolved deterministically.
- Future contributors must use requirement IDs and avoid restating canonical rules in multiple places.

## Alternatives Considered

- Multiple parallel indexes: rejected due to inevitable drift.
- “Everything is canonical”: rejected because it makes conflict resolution impossible.
