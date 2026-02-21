# Glossary

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This glossary defines terms used across the documentation. If a term is ambiguous, this file is canonical.

---

## Core Entities

**Snapshot**  
An immutable, persisted capture of market inputs used for analytics: underlying quote(s), option chain quote(s), rates/dividends/borrow inputs, and provenance metadata.

**Live Snapshot**  
A snapshot produced on a schedule (or by user action) that reflects the latest available provider data at its capture time. Each capture produces a new immutable snapshot.

**Pinned Snapshot**  
A snapshot selected by the user as the basis for analysis. While pinned, analytics MUST NOT silently switch to a newer snapshot.

**Scenario**  
A set of explicit overrides or transformations applied to a base snapshot for analytic evaluation (e.g., spot shift, vol shift, time shift, rate curve override).

**Trade Definition**  
A deterministic definition of a proposed trade: legs, quantities, instrument identifiers, and any trade-level parameters.

**Leg**  
A single instrument and quantity within a trade definition.

**Evidence Bundle**  
A portable artifact that allows third-party replay of outputs, containing snapshot, scenario, trade, configuration, engine version, and output hashes.

---

## Analytics Terms

**Price**  
Model price per contract (or per unit), with currency and multiplier rules defined in the analytics spec.

**IV (Implied Volatility)**  
Volatility implied by a target price under the model and conventions.

**Greeks**  
First/second order sensitivities. Units and scaling are defined in `docs/03_DOMAIN/ANALYTICS_SPEC.md`.

**Day Count Convention**  
The function mapping calendar time to year fractions, used in pricing and sensitivities.

---

## Determinism Terms

**Canonical Serialization**  
A stable representation (e.g., canonical JSON) used for hashing, ensuring order and formatting are deterministic.

**Deterministic ID**  
An identifier derived from canonical serialization of content (typically via a cryptographic hash) such that identical content yields identical IDs.

**Tolerance Envelope**  
A defined acceptance band for numerical comparisons in validation/parity contexts, defined in `docs/05_VALIDATION/TOLERANCES.md`.
