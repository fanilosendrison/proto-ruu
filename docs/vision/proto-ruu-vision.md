# proto-ruu Vision

> Non-normative direction. This document does not override the Product Intent,
> accepted ADRs, or future derived invariants.

`proto-ruu` is a transitional component intended to provide the routine Git
versioning layer of an agentic software-development system while broader
systems are not yet available.

Its role is to let a user or calling system hand over work whose boundary has
already been established and stop thinking about the routine Git versioning
operations required to durably represent that work in native Git and to
complete its direct-push publication.

The intended conceptual composition is:

```text
caller / user
→ establishes the WorkBoundary and the applicable authority

proto-ruu
→ performs the routine Git versioning progression of the supplied work
→ represents its effects as native Git state
→ completes the required direct-push publication

Git
→ remains the versioning substrate, publication boundary, and
  interoperability boundary
```

proto-ruu is deliberately bounded to publication that can be realized by
direct Git push. Richer publication routes (pull requests, merge queues,
stacked pull requests, provider-specific workflows, review-gated publication)
and general convergence are outside its domain; they belong to later systems.

proto-Go is an intended first-class caller of proto-ruu.

The standalone `/ruu` invocation is an intended ordinary coding-agent
experience and may supply only the best WorkBoundary available from its session
and Git context.

A dedicated Git worktree is not a proto-ruu prerequisite.

Ruu is the durable target system for broader agentic version control,
including concurrent convergence, richer publication and provider
realization, and a broader managed-state model. proto-ruu is not an
incremental step toward Ruu and must not progressively acquire Ruu's
responsibilities: its simplicity is intentional. When broader capabilities
become required, they belong to Ruu or another system rather than to
proto-ruu.

General concurrent convergence remains outside that boundary: when concurrent
change to a Git reality required by an invocation invalidates its progression,
proto-ruu blocks safely and returns control to the caller rather than
reconciling the contributions itself.

These relationships are directional context, not permission to import
proto-Go, Ruu, or git-commits-push architecture into proto-ruu.

The initial repository deliberately remains specification-first.

The immediate work is to derive the product model, obligations, invariants, and
open semantic decisions from the accepted Product Intent before selecting the
implementation architecture.
