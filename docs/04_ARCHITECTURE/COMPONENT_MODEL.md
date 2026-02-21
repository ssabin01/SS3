# Component Model

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines component responsibilities and state boundaries. It is canonical for “separation of concerns”.

---

## 1. Components and Responsibilities

### 1.1 UI Workstation (Stateful)

Owns:
- view state (selected snapshot, current trade draft, scenario draft)
- display preferences
- local UI cache (non-authoritative)

Must not own:
- authoritative snapshot storage
- analytics output truth without accompanying IDs/hashes

### 1.2 Provider Adapter (Stateless with I/O)

Owns:
- network calls to provider
- mapping provider payload → normalized records

Must not own:
- cached “market state” used for analytics (snapshots own that)

### 1.3 Snapshot Store (Stateful)

Owns:
- persistence of snapshots and raw payloads
- deterministic snapshot identity generation

Properties:
- append-only behavior preferred (immutability)
- supports querying by snapshot_id

### 1.4 Analytics Engine (Stateless given inputs)

Owns:
- pricing models
- IV solver
- greeks computation
- deterministic output hashing

Must not own:
- hidden, time-varying state that affects outputs

### 1.5 Audit Logger (Stateful append-only)

Owns:
- immutable audit events

---

## 2. Stateless vs Stateful Summary

| Component | Classification | Authoritative state |
|---|---|---|
| UI Workstation | Stateful | UI preferences and drafts only |
| Provider Adapter | Stateless (I/O) | none |
| Snapshot Store | Stateful | snapshots + provenance |
| Analytics Engine | Stateless (pure compute) | none (cache allowed with deterministic keys) |
| Audit Logger | Stateful | audit event log |

---

## 3. Boundary Rules

- UI ↔ Engine communications MUST use the interface contracts in `docs/04_ARCHITECTURE/INTERFACE_CONTRACTS.md`.
- Provider adapter MUST write through snapshot store; it cannot bypass snapshot identity generation.
- Analytics engine MUST only accept normalized snapshot inputs; it does not parse raw provider payloads.
