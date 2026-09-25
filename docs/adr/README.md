# proto-ruu Architecture Decision Records

This directory records explicit accepted decisions that establish, clarify, or
amend `proto-ruu` product semantics or architecture.

The normative Product Intent currently lives in:

```text
../specification/proto-ruu-spec.md
```

Accepted ADRs are:

- [ADR-001](adr-001-concurrency-does-not-imply-convergence.md) — proto-Ruu does
  not resolve concurrency between contributions.
- [ADR-002](adr-002-coordination-mechanism-undecided.md) — the concurrency
  coordination mechanism remains undecided; amends ADR-001.
- [ADR-003](adr-003-direct-push-publication-envelope.md) — the validity
  envelope is bounded to direct-push publication; confirms ADR-001.

Do not create an ADR merely because an implementation choice is convenient.

Create a semantic ADR only when an actual product-semantic question has been
identified and explicitly resolved by the product owner.

Create an architectural ADR only after the governing Product Intent and derived
invariants are sufficient to constrain that decision.

An unresolved product-semantic question must remain unresolved rather than being
silently decided by implementation.

Accepted ADRs record decision history. The synchronized normative specification
remains the current product-meaning projection.
