# Scaling Model

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines scaling assumptions and boundaries.

Sections marked **(Non-Binding)** describe future possibilities and do not override MVP requirements.

---

## 1. MVP Scaling Assumptions (Binding)

- Single-user workstation
- Single active snapshot evaluation stream at a time
- Local persistence (embedded store)
- Provider adapter operates under a single user credential context

---

## 2. Concurrency Model (Binding)

- Multiple evaluations MAY run concurrently, but MUST:
  - use deterministic cache keys
  - not share mutable state that affects outputs
  - produce identical outputs regardless of scheduling order

---

## 3. Future Server Mode (Non-Binding)

A server mode may introduce:

- shared snapshot store for teams
- centralized evaluation service
- entitlements and audit controls

However, server mode MUST preserve:

- snapshot immutability semantics
- deterministic identity and evidence bundle replayability
- contract versioning

---

## 4. Multi-User Considerations (Non-Binding)

If multi-user is introduced, additional controls are required:

- authentication and authorization
- tenant isolation
- data retention policies per tenant
- encrypted evidence bundles at rest and in transit

Those controls must be specified in future ADRs.
