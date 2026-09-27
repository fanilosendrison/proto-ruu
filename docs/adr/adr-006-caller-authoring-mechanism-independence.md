---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "proto-Ruu is independent of the caller's concrete managed-authoring mechanism"
id: "ADR-006"
status: "accepted"
date: "2026-09-27"
decision_body_sha256: "1bf08061d304fe0df9b38df161db03230455803a6f758744a38c61ae788dc501"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: []
  supersedes: []
  confirms: []
governs:
  - "independence of proto-ruu's product contract from the caller's concrete managed-authoring mechanism"
---

# ADR-006: proto-Ruu is independent of the caller's concrete managed-authoring mechanism

## Context

proto-Go is a first-class caller of proto-Ruu.

The earlier proto-Ruu definition still contained a passage stating that
proto-Go could provide dedicated temporary Git worktrees because its
managed-authoring semantics required them. That statement is no longer true.

proto-Go ADR-019 replaced the selection of a Git worktree mechanism with
abstract `Managed Authoring Environment` guarantees. That environment is
defined by its observable guarantees and does not select a VM, a container, a
process boundary, a filesystem, a Git worktree, a snapshot, a clone, an
image, a cache, a provider, or a provisioning mechanism.

That evolution of proto-Go must not become a new architectural dependency in
proto-Ruu.

The product question is therefore whether the proto-Ruu contract depends on
the concrete authoring mechanism used by its caller.

## Decision

proto-Ruu is independent of the caller's concrete managed-authoring mechanism.

Therefore:

- proto-Ruu MUST NOT require a Git worktree, VM, container, clone, filesystem
  arrangement, process boundary, snapshot, or any other concrete caller-owned
  authoring mechanism as a prerequisite;
- those mechanisms may exist outside proto-Ruu, but they do not become
  proto-Ruu product semantics merely because a caller uses them;
- proto-Ruu receives an applicable WorkBoundary and Git authority;
- the caller remains responsible for determining which authored work belongs
  to the supplied WorkBoundary;
- the concrete caller environment MUST NOT be treated as the WorkBoundary
  merely because it contains the work;
- visibility of additional repositories or mutable state inside a caller
  environment MUST NOT widen the supplied WorkBoundary;
- proto-Ruu's standalone `/ruu` case remains valid and does not require work
  to have been produced under proto-Go;
- this decision does not select the representation of WorkBoundary,
  observation scope, access mechanism, workspace materialization,
  VM/container/worktree mechanism, persistence mechanism, or handoff API.

```text
caller authoring environment != WorkBoundary
caller authoring mechanism != proto-Ruu prerequisite
```

## Alternatives considered

1. **Require the same concrete authoring mechanism as proto-Go.**
   Rejected because it would couple proto-Ruu to a caller-owned implementation
   mechanism and make future caller changes alter proto-Ruu without changing
   its own product intent.

2. **Treat the complete caller environment as the WorkBoundary.**
   Rejected because environment visibility does not establish work ownership
   or mutation authority.

3. **Keep the current worktree-specific wording.**
   Rejected because it now states a false premise about proto-Go and
   unnecessarily frames proto-Ruu around one possible mechanism.

## Consequences

- proto-Ruu remains usable with proto-Go regardless of the concrete Managed
  Authoring Environment realization;
- proto-Ruu remains usable standalone;
- WorkBoundary remains the semantic work limit presented to proto-Ruu;
- future VM/container/worktree choices remain caller/integration architecture
  and not proto-Ruu Product Intent;
- the exact access and handoff mechanisms remain future derivations;
- no change is made to the direct-push validity envelope;
- no change is made to ADR-001/002 concurrency semantics;
- no change is made to proto-Go publication ownership.

## References

- `docs/specification/proto-ruu-spec.md`
- `docs/vision/proto-ruu-vision.md`
- `README.md`
