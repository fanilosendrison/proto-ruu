# proto-ruu Engineering Governance

This document governs repository work classification and authority boundaries.

It does not define `proto-ruu` product semantics.

## Authority

Use each source only for the responsibility it owns.

1. `docs/specification/proto-ruu-spec.md`
   owns current normative product meaning.

2. Accepted ADRs under `docs/adr/`
   record explicit decision history and later amendments.

3. `docs/vision/proto-ruu-vision.md`
   is non-normative motivation and long-term direction.

4. `docs/repository-governance/`
   owns repository procedure, not product meaning.

5. Future formal artifacts may verify derived properties for their declared
   scope but must not become independent Product Intent authority.

6. Future implementation and tests must conform to upstream product authority;
   they must not silently define missing semantics.

## Current repository phase

The repository is currently in:

```text
PRODUCT DEFINITION
```

The current normative product meaning is the accepted Product Intent in:

```text
docs/specification/proto-ruu-spec.md
```

Accepted ADRs under `docs/adr/` record explicit decision history and
amendments.

No complete invariant set exists yet.

No canonical terminology section has been derived yet.

No implementation architecture has been accepted yet.

No implementation language has been selected yet.

No process model has been selected yet.

No persistence mechanism has been selected yet.

No orchestration runtime has been selected yet.

No locking strategy has been selected yet.

No commit-planning algorithm has been selected yet.

No branch topology has been selected yet.

No worktree topology has been selected yet.

No remote provider has been selected yet.

No recovery mechanism has been selected yet.

## Work classes

Classify repository work into these responsibility classes:

```text
Product Semantics
Formal Assurance
Architecture
Implementation
Qualification
Repository Governance
```

### Product Semantics

Use for work that establishes, derives, clarifies, or amends product meaning.

This includes:

* Product Intent changes;
* normative semantic decisions;
* canonical terminology;
* invariant derivation;
* semantic ADRs;
* specification changes.

### Formal Assurance

Use only after formal-assurance responsibilities have been explicitly
established.

Formal work checks accepted semantics.

It does not invent them.

### Architecture

Use only when upstream product semantics and invariants are sufficient to
constrain a mechanism-level design decision.

Architecture must not silently close a `decision-required` product question.

### Implementation

Use only to realize accepted architecture and semantics.

Implementation convenience never outranks Product Intent.

### Qualification

Use for evidence that an implementation satisfies the accepted product
contract.

Qualification does not define the contract.

### Repository Governance

Use for repository procedure, work management, artifact ownership, validation,
and contribution mechanics.

Repository governance does not define `proto-ruu` behavior for users.

## Current sequencing rule

Until further explicit product derivation occurs:

```text
Product Intent
→ semantic questions
→ explicit decisions where required
→ derived invariants
→ formal/assurance design as required
→ architecture
→ implementation
→ qualification
```

Do not skip from Product Intent directly to speculative implementation.

## Existing-project non-authority

The following projects may be consulted later as implementation context but are
not semantic authority for `proto-ruu`:

```text
Turnlock
proto-Go
Ruu
git-commits-push
dotagents
permission-enforcer
```

Do not copy their architecture into `proto-ruu` unless a later accepted
`proto-ruu` derivation independently requires it.
