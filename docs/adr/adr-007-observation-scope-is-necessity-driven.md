---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "proto-Ruu observation scope is necessity-driven and does not expand authority"
id: "ADR-007"
status: "accepted"
date: "2026-09-27"
decision_body_sha256: "da095f64dca4731a276ef1a50b49d60aa6db687a9e397d4ab1b4ed1f354fd369"
relation_completeness: "complete"
relations:
  clarifies: ["ADR-006"]
  amends: []
  supersedes: []
  confirms: []
governs:
  - "Git reality proto-ruu may observe while progressing a supplied WorkBoundary"
  - "separation between observation, WorkBoundary membership, and mutation authority"
---

# ADR-007: proto-Ruu observation scope is necessity-driven and does not expand authority

## Context

proto-Ruu receives a WorkBoundary and Git authority.

The WorkBoundary bounds the work proto-Ruu takes charge of.

A too-restrictive interpretation would treat the WorkBoundary as the totality
of the Git reality proto-Ruu is allowed to observe. That interpretation would
be incorrect because a safe Git progression may require observing facts that
are not themselves the work taken charge of.

The inverse interpretation would also be incorrect: allowing open-ended
exploration of the environment in order to discover additional work.

What governs proto-Ruu's Git observation therefore has to be decided.

## Decision

proto-Ruu's Observation Scope is necessity-driven rather than defined as a
fixed extensional list.

proto-Ruu MAY observe Git reality outside the supplied WorkBoundary only when
that observation is necessary to determine one or more of:

- the current Git state of the supplied work;
- the safety of an authorized version-control progression;
- whether an authorized progression or Git effect is already satisfied.

Observation MUST NOT by itself:

- add work to the WorkBoundary;
- establish work ownership;
- establish authored provenance;
- grant mutation authority;
- authorize adoption of another contribution;
- authorize global or ambient work discovery.

```text
observation != ownership
observation != WorkBoundary membership
observation != mutation authority
```

The governing question is:

"What Git reality is necessary to determine or safely realize the authorized
progression of THIS supplied WorkBoundary?"

not:

"What other work exists that proto-Ruu could take responsibility for?"

The Product Intent does not enumerate a closed list of readable Git entities.

A future implementation may obtain broader raw data than the semantic question
strictly requires when a Git mechanism returns such data, but the availability
of that data does not make every returned entity semantically relevant,
work-bearing, owned, or mutable by proto-Ruu.

This decision does not select:

```text
Git command set
observation algorithm
remote-fetch strategy
ref-enumeration strategy
worktree-enumeration strategy
object traversal algorithm
cache
persistence
network protocol
provider API
WorkBoundary representation
Git-authority representation
```

## Alternatives considered

1. **Fixed observation whitelist.**
   The concept is to define a closed list such as HEAD / index / working tree /
   target ref.
   Rejected: a frozen product list would risk forbidding a later Git
   observation that is necessary to correctly establish the safety or
   satisfaction of a progression, even though the product responsibility has
   not actually changed.

2. **Observation physically restricted to the WorkBoundary.**
   Rejected: the WorkBoundary bounds the work taken charge of, not necessarily
   every Git fact that must be read to progress that work safely.

3. **Open-ended ambient discovery.**
   The concept is to scan whatever Git state is available and infer additional
   work to handle.
   Rejected: that would reintroduce global discovery, work-ownership inference,
   and adoption of additional work, responsibilities proto-Ruu does not have.

## Consequences

### Benefits

- proto-Ruu can observe the facts actually necessary for a safe progression
  without turning the WorkBoundary into a read sandbox.
- The WorkBoundary remains the semantic limit of the work taken charge of.
- Observing concurrency or remote state remains possible without adopting the
  observed contribution.
- The Product Intent does not depend on a fragile list of Git primitives.

### Costs and obligations

- A future derivation must determine which concrete observations are necessary
  for the different progression cases.
- Each observation must be justifiable by a progression question of the
  supplied work.
- Implementation cannot turn the availability of Git information into work
  ownership.
- The concrete observation strategy remains undecided.

## References

- ADR-001: proto-Ruu does not resolve concurrency between contributions.
- ADR-006: proto-Ruu is independent of the caller's concrete managed-authoring
  mechanism.
- `docs/specification/proto-ruu-spec.md`
- `README.md`
- `docs/vision/proto-ruu-vision.md`
