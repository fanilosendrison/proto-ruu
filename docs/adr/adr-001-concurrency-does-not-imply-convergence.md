---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "proto-Ruu does not resolve concurrency between contributions"
id: "ADR-001"
status: "accepted"
date: "2026-09-25"
decision_body_sha256: "e91c33762eb72b341ff51f4da74aab90a69988ce873bd62fc3066b0f05dadf6a"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: []
  supersedes: []
  confirms: []
governs:
  - "concurrency encountered while proto-ruu progresses a supplied WorkBoundary"
  - "behavior when required Git preconditions are invalidated by a concurrent contribution"
  - "ownership boundary between proto-ruu progression and caller-side continuation"
  - "distinction between proto-ruu bounded progression and Ruu convergence"
---

# ADR-001: proto-Ruu does not resolve concurrency between contributions

## Context

proto-Ruu progresses a supplied WorkBoundary toward durable Git state and,
when the invocation contract requires it, authorized publication. Multiple
proto-Ruu invocations may exist and progress concurrently. Two independent
proto-Go contributions, for example, may each supply their own WorkBoundary
while both must publish toward the same Git reality.

Each invocation observes the Git preconditions it requires. One invocation can
advance that shared Git reality while another invocation is still operating
against the state it observed:

```text
proto-Go A -> WorkBoundary A -> proto-Ruu A
proto-Go B -> WorkBoundary B -> proto-Ruu B

remote = M

proto-Ruu A observes M
proto-Ruu B observes M

A publishes: M -> A1

proto-Ruu B: expected remote = M, actual remote = A1
```

The second invocation's required preconditions are then invalidated. The
product question is whether proto-Ruu must reconcile that change (merge,
rebase, reconcile, force-push, adopt the other contribution), or stop and
return control to the caller. Without an explicit product decision, a
conforming implementation could silently drift toward general convergence
behavior that belongs to Ruu.

## Decision

proto-Ruu does not resolve concurrency between contributions.

proto-Ruu safely progresses the WorkBoundary supplied to it. If, during that
progression, a Git reality required by the operation changes in a way that
makes the requested effect no longer demonstrably safe under the authority the
invocation holds — for example because another contribution advanced the same
remote destination — proto-Ruu MUST NOT merge, rebase, reconcile, force-push,
or otherwise absorb the concurrency. It MUST stop safely and return to the
caller a result indicating that progression is blocked by the observed
concurrent Git reality.

proto-Ruu MAY detect concurrent change. Detection of concurrent change MUST NOT
become general convergence ownership.

Independent proto-Ruu invocations may progress concurrently when
non-conflicting. proto-Ruu MUST NOT require global serialization of independent
WorkBoundaries. An invocation acts only under the authority and Git assumptions
applicable to its own WorkBoundary.

The caller remains responsible for deciding how its objective continues — for
example, preserving the same managed-contribution objective, retiring prior
readiness authority if authored mutation must resume, requesting authored
correction or convergence, revalidating affected work, establishing a later
readiness occurrence, and invoking proto-Ruu again. proto-Ruu MUST NOT know or
own that caller-side continuation logic.

This record does not decide the exact blocked-result shape, an API enum or
schema, a locking mechanism, CAS versus lease or another Git protocol, lock
granularity, a retry strategy, the representation of a Git precondition, a
collision-detection algorithm, or a persistence model. Those remain future
derivations.

## Alternatives considered

- **Resolve concurrency inside proto-Ruu (merge, rebase, conflict resolution,
  force-push, adoption of another contribution, general convergence):**
  rejected because it converts detection of concurrent change into convergence
  ownership, exceeds the authority supplied with the WorkBoundary, and
  silently absorbs caller-owned semantics.
- **Serialize all proto-Ruu invocations globally:** rejected because
  independent WorkBoundaries are allowed to progress concurrently; the
  required property is safe blocking of an invocation whose preconditions were
  invalidated, not "only one proto-Ruu invocation may exist at a time".

## Consequences

### Benefits

- proto-Ruu remains a bounded safe-progression layer and never silently
  overwrites, merges, or adopts work outside its authority.
- Blockage caused by concurrent Git reality becomes an observable outcome
  returned to the caller rather than an implicit resolution.
- Independent WorkBoundaries keep progressing in parallel while compatible.
- The product boundary between proto-Ruu and Ruu stays explicit: detecting
  concurrency is not owning convergence.

### Costs and obligations

- The caller must handle blocked outcomes and own any continuation strategy
  (work correction, authority renewal, readiness re-establishment, authored
  convergence).
- The blocked-result shape, Git precondition representation, collision
  detection, locking or compare-and-swap protocol, retry strategy, and
  persistence model remain future derivations.
- Concurrent progression relies on Git behavior and observation rather than on
  a global coordinator; incompatibilities must surface as blocked outcomes
  instead of being reconciled.

## References

- `docs/specification/proto-ruu-spec.md`
- `docs/vision/proto-ruu-vision.md`
- `docs/adr/README.md`
- `docs/adr/index.md`
- `docs/repository-governance/proto-ruu-discovery-classification.md`
- `docs/repository-governance/proto-ruu-engineering.md`
