---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "The concurrency coordination mechanism remains undecided"
id: "ADR-002"
status: "accepted"
date: "2026-09-25"
decision_body_sha256: "f957dd90858c138b4c18858b788cf8fd260365fad331151c85d8333895c03bad"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: ["ADR-001"]
  supersedes: []
  confirms: []
governs:
  - "scope of the no-global-serialization property recorded by ADR-001"
  - "undecided status of any coordination mechanism for concurrent proto-ruu progression"
---

# ADR-002: The concurrency coordination mechanism remains undecided

## Context

ADR-001 records that proto-Ruu does not resolve concurrency between
contributions: independent invocations may progress concurrently, and an
invocation whose required Git preconditions are invalidated must block safely
and return control to its caller. ADR-001 also explicitly lists the locking
mechanism, CAS-versus-lease protocol, lock granularity, and persistence model
as undecided.

One sentence in ADR-001's Consequences states that "concurrent progression
relies on Git behavior and observation rather than on a global coordinator."
Read literally, that sentence excludes an internal coordinator, registry, or
lock service, which goes beyond the accepted decision. The accepted decision
prohibits a requirement for global serialization; it does not prohibit a
coordination mechanism, and it explicitly leaves that mechanism undecided.
Product-owner review of the published ADR-001 identified this overreach.

## Decision

Concurrent progression must preserve safe detection of incompatible Git-state
change without requiring global serialization of independent WorkBoundaries.

The coordination mechanism remains undecided at the product level. This
includes the possible existence of an internal coordinator, registry, lock
service, or another mechanism: none is required, and none is excluded.

ADR-001's Consequences phrasing must not be read as prohibiting such a
mechanism. Only the undecided status and the no-required-serialization property
are normative.

## Alternatives considered

- **Read ADR-001 literally as prohibiting an internal coordinator:** rejected
  because the accepted decision only prohibits a requirement for global
  serialization and explicitly leaves the coordination mechanism undecided.
- **Leave ADR-001 unchanged without an amendment:** rejected because an
  accepted record must not carry an implication that contradicts its own scope.
- **Rewrite ADR-001 in place:** rejected because accepted ADR bodies are
  immutable; decision history is preserved through an amending record.

## Consequences

### Benefits

- Keeps the product boundary identical while preserving architectural space for
  a future coordination mechanism.
- Removes an accidental mechanism decision that ADR-001 itself declared
  undecided.

### Costs and obligations

- The corpus carries an additional ADR; readers must follow the amendment
  relation to read ADR-001 correctly.
- ADR-001's original sentence remains in the immutable historical record and is
  corrected only through this amendment.
- Any future coordination mechanism must still preserve ADR-001's safe-blocking
  behavior.

## References

- ADR-001: proto-Ruu does not resolve concurrency between contributions.
- `docs/specification/proto-ruu-spec.md`
- `docs/adr/README.md`
- `docs/adr/index.md`
