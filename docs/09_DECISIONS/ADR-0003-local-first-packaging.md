# ADR-0003: Local-First Packaging for MVP

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


**Decision status:** Accepted  
**Date:** 2026-02-19

## Context

MVP prioritizes speed of iteration, deterministic local compute, and minimization of operational complexity. Hedge fund environments can restrict outbound connectivity and require local operation.

## Decision

1. MVP is local-first.
2. UI + services may run as one process or multiple local processes.
3. Regardless of topology, interface contracts are enforced and evidence bundles are exportable.

## Consequences

- No dependency on a central server for core analytics.
- Future server mode is possible but requires additional ADRs for security/tenancy.

## Alternatives Considered

- Cloud-first MVP: rejected due to institutional deployment friction and increased security scope.
