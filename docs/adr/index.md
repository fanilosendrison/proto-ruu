# proto-ruu ADR Index

| ADR | Decision | Status |
| --- | -------- | ------ |
| [ADR-001](adr-001-concurrency-does-not-imply-convergence.md) | proto-Ruu does not resolve concurrency between contributions | accepted |
| [ADR-002](adr-002-coordination-mechanism-undecided.md) | The concurrency coordination mechanism remains undecided (amends ADR-001) | accepted |
| [ADR-003](adr-003-direct-push-publication-envelope.md) | proto-Ruu's validity envelope is bounded to direct-push publication (confirms ADR-001) | accepted |
| [ADR-004](adr-004-transitional-target-stack.md) | The transitional target stack is Turnlock, Go, and Ruu; proto-Go is an intended present first-class caller (amends ADR-003) | accepted |
| [ADR-005](adr-005-original-wording-correction.md) | ADR-003's immutable body retains the original wording; ADR-004 carries the correction (amends ADR-004) | accepted |

The current normative starting authority is
[`../specification/proto-ruu-spec.md`](../specification/proto-ruu-spec.md).

Do not add an ADR row until the corresponding ADR actually exists and has been
explicitly accepted.
