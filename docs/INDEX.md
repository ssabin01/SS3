# Documentation Index

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This directory is the **single entry point** for all product documentation. It defines:

- What each document is for
- Which documents are **normative** (binding) vs **informative**
- The **precedence order** to resolve conflicts deterministically

If you are onboarding, start here, then read in the order below.

---

## Precedence Order (Conflict Resolution)

When two documents conflict, resolve in this order (highest wins):

1. **ADR (Architectural Decision Records)** in `docs/09_DECISIONS/` (newer ADR wins over older ADR)
2. **Requirements** in `docs/02_REQUIREMENTS/REQUIREMENTS.md`
3. **Domain specs** in `docs/03_DOMAIN/` (math, market data conventions)
4. **Architecture specs** in `docs/04_ARCHITECTURE/`
5. **Validation specs** in `docs/05_VALIDATION/`
6. **Security & governance** in `docs/06_SECURITY_GOVERNANCE/`
7. **Operations** in `docs/07_OPERATIONS/`
8. **Roadmap/state tracking** in `docs/08_ROADMAP/` (non-normative; never overrides normative docs)

If a conflict cannot be resolved by precedence, it requires a new ADR.

---

## Document Map

### 01 Product

- `docs/01_PRODUCT/PRODUCT_BRIEF.md` — one-page definition
- `docs/01_PRODUCT/PRD.md` — product requirements document (goals, users, problem framing)
- `docs/01_PRODUCT/SCOPE.md` — scope boundaries and phased delivery
- `docs/01_PRODUCT/GLOSSARY.md` — terms and units used across the system

### 02 Requirements

- `docs/02_REQUIREMENTS/REQUIREMENTS.md` — functional + non-functional requirements (**canonical**)
- `docs/02_REQUIREMENTS/USER_FLOWS.md` — user-facing flows (derived from requirements)
- `docs/02_REQUIREMENTS/FAILURE_POLICY.md` — explicit failure handling policy (**canonical for failures**)
- `docs/02_REQUIREMENTS/PERFORMANCE_BUDGETS.md` — latency/throughput budgets and constraints

### 03 Domain

- `docs/03_DOMAIN/ANALYTICS_SPEC.md` — option analytics definitions, outputs, limitations
- `docs/03_DOMAIN/NUMERICAL_CONVENTIONS.md` — determinism, rounding, tolerances semantics
- `docs/03_DOMAIN/RATES_DIVIDENDS_BORROW.md` — how rates/dividends/borrow are sourced and applied
- `docs/03_DOMAIN/TOS_PARITY_SPEC.md` — external parity mode definition and constraints
- `docs/03_DOMAIN/DATA_SOURCES.md` — data provenance, licensing assumptions, required metadata

### 04 Architecture

- `docs/04_ARCHITECTURE/ARCHITECTURE.md` — system-level architecture
- `docs/04_ARCHITECTURE/COMPONENT_MODEL.md` — responsibilities, boundaries, statefulness
- `docs/04_ARCHITECTURE/INTERFACE_CONTRACTS.md` — UI/service contract and internal interfaces
- `docs/04_ARCHITECTURE/DATA_MODEL.md` — logical schema and entity relationships
- `docs/04_ARCHITECTURE/SNAPSHOT_MODEL.md` — snapshot semantics and lifecycle (**canonical for snapshots**)
- `docs/04_ARCHITECTURE/STATE_AND_IDENTITY.md` — IDs, hashes, canonical serialization
- `docs/04_ARCHITECTURE/SCALING_MODEL.md` — scaling assumptions and growth path (non-binding where marked)

### 05 Validation

- `docs/05_VALIDATION/VALIDATION_STRATEGY.md` — validation plan and test taxonomy
- `docs/05_VALIDATION/GOLDEN_CASES.md` — golden datasets format and coverage plan
- `docs/05_VALIDATION/TOLERANCES.md` — tolerance definitions and acceptance rules
- `docs/05_VALIDATION/REPRODUCIBILITY.md` — replay/audit evidence requirements

### 06 Security & Governance

- `docs/06_SECURITY_GOVERNANCE/SECURITY_MODEL.md` — security posture, secrets, and threat assumptions
- `docs/06_SECURITY_GOVERNANCE/DATA_GOVERNANCE.md` — retention, provenance, classification
- `docs/06_SECURITY_GOVERNANCE/AUDIT_LOGGING.md` — audit event schema and requirements

### 07 Operations

- `docs/07_OPERATIONS/DEPLOYMENT.md` — packaging and deployment model(s)
- `docs/07_OPERATIONS/RUNBOOK.md` — operator procedures for local + optional server mode
- `docs/07_OPERATIONS/OBSERVABILITY.md` — logs/metrics/traces requirements
- `docs/07_OPERATIONS/INCIDENT_RESPONSE.md` — incident handling playbook

### 08 Roadmap & State (Non-Normative)

- `docs/08_ROADMAP/ROADMAP.md`
- `docs/08_ROADMAP/STATE.md`

### 09 Decisions

- `docs/09_DECISIONS/ADR-0001-documentation-governance.md`
- `docs/09_DECISIONS/ADR-0002-determinism-and-snapshots.md`
- `docs/09_DECISIONS/ADR-0003-local-first-packaging.md`
- `docs/09_DECISIONS/ADR-0004-analytics-engine-and-numerics.md`

---

## Normative Vocabulary

Across normative docs:

- **MUST** / **MUST NOT** = required for compliance
- **SHOULD** / **SHOULD NOT** = strongly recommended; deviations require explicit justification
- **MAY** = optional
