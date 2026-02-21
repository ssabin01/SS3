# ToS Parity Specification

**Status:** Normative  
**Version:** 0.1  
**Last updated:** 2026-02-19


This document defines **ToS Parity Mode**.

Parity mode exists to reproduce analytics from an external reference tool (e.g., Thinkorswim) to within a defined tolerance envelope.

Parity mode is **not** the default analytics mode. It is an explicit configuration and must be versioned.

---

## 1. Definitions

**Reference Tool**  
The external tool whose outputs are treated as the comparison baseline.

**Parity Version**  
A version identifier capturing:
- reference tool identity and version
- reference extraction method (manual/automation)
- data mapping rules
- tolerance envelope

---

## 2. Parity Contract

Given an input set `I`:

- snapshot-equivalent market inputs
- scenario-equivalent overrides
- trade definition

The parity contract is:

- AegisRisk outputs `O_aegis`
- Reference tool outputs `O_ref`

Parity is satisfied if:

`distance(O_aegis, O_ref) <= tolerance(metric, instrument_class, output_type)`

Tolerance definitions are canonical in `docs/05_VALIDATION/TOLERANCES.md`.

---

## 3. Evidence Requirements

Each parity validation record MUST store:

- reference tool version metadata
- reference outputs captured (raw)
- mapping version used to align reference inputs/outputs to AegisRisk schema
- the tolerance envelope applied
- pass/fail per output dimension

---

## 4. Drift Handling

Reference tools change over time. Therefore:

- Parity mode MUST be explicitly versioned.
- A new parity version MUST be created when:
  - reference tool version changes materially, or
  - mapping rules change, or
  - tolerance envelope changes

No silent parity “retuning” is allowed.

---

## 5. Limitations

- If reference tool does not expose all inputs (rates/dividends/borrow), parity is scoped to the inputs that can be aligned and the system MUST record which inputs were not aligned.
- Parity does not imply model correctness; only agreement within defined tolerances.
