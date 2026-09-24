# proto-ruu Vision

> Non-normative direction. This document does not override the Product Intent,
> accepted ADRs, or future derived invariants.

`proto-ruu` is intended to become the routine Git versioning layer of an
agentic software-development system.

Its long-term role is to let a user or calling system hand over work whose
boundary has already been established and stop thinking about the routine Git
versioning operations required to durably represent that work and advance it
across the authorized Git boundary.

The intended conceptual composition is:

```text
caller / user
→ establishes the WorkBoundary and the applicable authority

proto-ruu
→ performs the routine Git versioning progression of the supplied work
→ represents its effects as native Git state

Git
→ remains the versioning substrate and interoperability boundary
```

proto-Go is an intended first-class caller of proto-ruu.

The standalone `/ruu` invocation is an intended ordinary coding-agent
experience and may supply only the best WorkBoundary available from its session
and Git context.

A dedicated Git worktree is not a proto-ruu prerequisite.

Ruu is the broader system for general agentic Git convergence and
version-control progression. proto-ruu deliberately reproduces only a bounded
form of the Ruu user experience and does not inherit Ruu's global work
discovery, ownership inference, or unrelated-work reconciliation.

These relationships are directional context, not permission to import
proto-Go, Ruu, or git-commits-push architecture into proto-ruu.

The initial repository deliberately remains specification-first.

The immediate work is to derive the product model, obligations, invariants, and
open semantic decisions from the accepted Product Intent before selecting the
implementation architecture.
