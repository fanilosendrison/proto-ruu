---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "ADR-003's immutable body retains the original wording; ADR-004 carries the correction"
id: "ADR-005"
status: "accepted"
date: "2026-09-25"
decision_body_sha256: "c704be2a28f8995ed75091db948073f939f7fa874f6baaec2998da7a78b9cd9f"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: ["ADR-004"]
  supersedes: []
  confirms: []
governs:
  - "accuracy of the description of which record carries the correction"
---

# ADR-005: ADR-003's immutable body retains the original wording; ADR-004 carries the correction

## Context

ADR-004's "Costs and obligations" states:

> ADR-003 continues to contain the corrected wording in its immutable body;
> readers must follow the amendment relation.

That statement is inverted. ADR-003 continues to contain the original incorrect
wording in its immutable body; ADR-004 is the record that carries the
correction. A reader following the amendment relation must therefore read
ADR-003 together with ADR-004, not ADR-003 alone.

The error is documentary and does not change the decision recorded by ADR-004.
Because ADR-004 is accepted and its body is immutable, the accurate statement
is recorded in a new amending record.

## Decision

ADR-003 continues to contain the original incorrect wording in its immutable
body. ADR-004 carries the correction, and readers must follow the amendment
relation.

Nothing else in ADR-004 changes.

## Alternatives considered

- **Leave ADR-004 unchanged:** rejected because the immutable corpus would
  keep a statement that says the opposite of the truth.
- **Rewrite ADR-004 in place:** rejected because accepted ADR bodies are
  immutable; decision history is preserved through an amending record.

## Consequences

### Benefits

- The corpus states accurately which record carries the correction.
- Readers of ADR-003 are directed to ADR-004 instead of being told that
  ADR-003 already contains corrected wording.

### Costs and obligations

- The corpus carries an additional ADR for a documentary correction.
- ADR-004 retains the inverted sentence in its immutable body; this record is
  the authoritative correction.

## References

- ADR-003: proto-Ruu's validity envelope is bounded to direct-push publication.
- ADR-004: The transitional target stack is Turnlock, Go, and Ruu; proto-Go is
  an intended present first-class caller.
