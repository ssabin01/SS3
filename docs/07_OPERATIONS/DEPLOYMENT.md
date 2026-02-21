# Deployment

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines deployment and packaging models.

---

## 1. MVP Deployment Model (Binding)

MVP is local-first.

- UI workstation runs on a user machine.
- Snapshot store persists locally.
- Provider adapter uses user-scoped credentials stored in OS keychain or encrypted vault.
- Analytics engine executes locally.

---

## 2. Process Topology (Binding)

The implementation MAY choose:

- single process (UI + engine embedded), or
- multiple local processes (UI + local service + engine)

Regardless of process model, interface contracts in `docs/04_ARCHITECTURE/INTERFACE_CONTRACTS.md` MUST be enforced.

---

## 3. Configuration

Configuration MUST support:

- selecting provider adapter and its environment
- retention policy settings
- export policy settings (internal-only vs redacted)
- enabling/disabling parity mode
- selecting engine/model/conventions versions (where multiple exist)

Configuration must be part of the evidence bundle when it affects outputs.

---

## 4. Server Mode (Non-Binding)

A server mode is optional and requires additional ADRs covering:

- authentication/authorization
- tenant isolation
- encryption and key management
- observability and incident response obligations
