# Security Model

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines the security posture for a hedge-fund-facing analytics workstation.

---

## 1. Security Objectives

S1. Protect provider credentials and access tokens  
S2. Prevent leakage of sensitive market data and proprietary scenarios/trades  
S3. Provide auditability of access and exports  
S4. Minimize attack surface in local-first deployment

---

## 2. Threat Assumptions

- Workstation may be on an untrusted network.
- Provider tokens may grant access to sensitive account-linked data.
- Evidence bundles may be shared; redistribution must be controlled.

---

## 3. Secrets Handling

- Provider credentials/tokens MUST NOT be stored in plaintext.
- Secrets MUST be stored in an OS keychain or an encrypted vault.
- Secrets MUST NOT be written to logs.
- Secrets MUST be scoped to least privilege and rotated per organizational policy.

---

## 4. Data at Rest

- Snapshot store data SHOULD be encrypted at rest.
- Raw provider payloads MUST be encrypted if persisted.

Encryption key management must be documented for the chosen storage approach.

---

## 5. Data in Transit

If components communicate over IPC/HTTP:

- local transport MUST be restricted to loopback by default
- TLS is REQUIRED for any non-local network transport
- request authentication is REQUIRED for server mode

---

## 6. Logging and Redaction

Logs MUST:

- redact tokens, account identifiers, and PII
- avoid logging raw provider payloads unless explicitly enabled for debugging with secure handling
- include trace IDs and deterministic context IDs (snapshot_id, trade_id, scenario_id)

---

## 7. Supply Chain and Build Integrity

- Dependencies MUST be pinned and reproducibly buildable.
- Release artifacts SHOULD be signed.
- Engine binaries referenced by evidence bundles MUST be identifiable (hash/signature).
