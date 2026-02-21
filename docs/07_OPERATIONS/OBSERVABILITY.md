# Observability

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines observability requirements.

---

## 1. Logging

Logs MUST:

- include a trace ID per operation
- include relevant context IDs:
  - snapshot_id, trade_id, scenario_id, evaluation_id
- be structured (JSON preferred)
- redact secrets and PII

---

## 2. Metrics

Minimum metrics:

### Provider Adapter
- snapshot creation success/failure counts by provider_id
- provider latency distribution
- auth failure counts

### Analytics Engine
- evaluation latency distribution
- cache hit rate (if cache used)
- solver iteration distributions and convergence failure counts

### Storage
- snapshot store size
- read/write latency
- retention purge counts

---

## 3. Tracing (Optional MVP)

If tracing is implemented:

- spans must include context IDs
- export must include trace IDs for audit correlation

---

## 4. Privacy and Governance

Observability data is subject to governance and retention rules in `docs/06_SECURITY_GOVERNANCE/DATA_GOVERNANCE.md`.
