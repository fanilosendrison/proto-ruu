---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "The transitional target stack is Turnlock, Go, and Ruu; proto-Go is an intended present first-class caller"
id: "ADR-004"
status: "accepted"
date: "2026-09-25"
decision_body_sha256: "5b738ee68b76a12a76c977c034e85654b1c8098742647c8364277b8ff3779d21"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: ["ADR-003"]
  supersedes: []
  confirms: []
governs:
  - "the transitional reference stack for proto-ruu"
  - "proto-Go's role as an intended present first-class caller"
---

# ADR-004: The transitional target stack is Turnlock, Go, and Ruu; proto-Go is an intended present first-class caller

## Context

ADR-003 states that "proto-Ruu is a transitional component used while Turnlock,
proto-Go, and Ruu are not available." The Product Intent likewise named
"Turnlock, proto-Go, and Ruu" as the systems whose absence proto-Ruu bridges.

That identification is wrong. It conflates two different names:

```text
proto-Go
    an intended present first-class caller of proto-Ruu
    (section 0.5 of the Product Intent)

Go
    a member of the future target stack
    named in the product-owner decision as
    Turnlock + Go + Ruu
```

proto-Go is not a future system whose absence justifies proto-Ruu; it is one
of the systems that consumes proto-Ruu now. The actual product model is:

```text
now

proto-Go
   ↓
proto-Ruu
   ↓
direct-push repositories
```

and later:

```text
Turnlock + Go + Ruu
```

The error contradicts section 0.5's first-class-caller relationship and the
product-owner decision.

## Decision

proto-Ruu is transitional relative to the future Turnlock + Go + Ruu target
stack.

proto-Go is NOT one of the systems whose absence justifies proto-Ruu. proto-Go
is an intended present first-class caller of proto-Ruu and one of the systems
that consumes proto-Ruu now.

ADR-003's naming of the absent systems is amended accordingly. Nothing else in
ADR-003 changes: the direct-push validity envelope, the conceptual
SUCCESS/BLOCKED/OUT OF SCOPE classes, the preserved concurrency rule, and the
non-evolution toward Ruu remain as decided.

## Alternatives considered

- **Leave ADR-003 unchanged:** rejected because it contradicts the
  first-class-caller model in the Product Intent and the product-owner
  decision.
- **Rewrite ADR-003 in place:** rejected because accepted ADR bodies are
  immutable; decision history is preserved through an amending record.

## Consequences

### Benefits

- The transitional model distinguishes the present caller (proto-Go) from the
  future target stack (Turnlock, Go, Ruu).
- No reader can mistake proto-Go for a future system that proto-Ruu is waiting
  for.

### Costs and obligations

- ADR-003 continues to contain the corrected wording in its immutable body;
  readers must follow the amendment relation.
- Any future reference to the transitional stack must use Turnlock + Go + Ruu
  and must not include proto-Go.

## References

- ADR-003: proto-Ruu's validity envelope is bounded to direct-push publication.
- `docs/specification/proto-ruu-spec.md`
- `docs/vision/proto-ruu-vision.md`
- `README.md`
