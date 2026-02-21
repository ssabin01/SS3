# Incident Response

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines incident response procedures. MVP is local-first, but incidents still matter for institutional clients.

---

## 1. Severity Levels

- **SEV-1**: security incident or data exfiltration risk
- **SEV-2**: deterministic correctness failure affecting outputs
- **SEV-3**: availability failure (cannot create snapshots / evaluate)
- **SEV-4**: performance degradation beyond budgets

---

## 2. Required Immediate Actions

### SEV-1
- disable exports if there is leakage risk
- rotate credentials and revoke tokens
- preserve forensic evidence (logs/audit)

### SEV-2
- freeze affected engine_version from distribution
- preserve evidence bundles demonstrating mismatch
- run replay validation to localize drift source

### SEV-3
- collect provider adapter logs and status
- verify storage health and permissions
- provide fallback: pinned snapshot evaluation offline (if possible)

---

## 3. Postmortem Requirements

A postmortem MUST include:

- incident timeline
- impact assessment (what outputs/users were affected)
- root cause
- corrective actions (tracked via ADR where appropriate)
- validation updates required to prevent recurrence
