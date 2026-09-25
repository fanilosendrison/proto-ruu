# proto-ruu

`proto-ruu` is the routine Git versioning boundary operating over a supplied
WorkBoundary.

The user-facing invocation of proto-ruu is `/ruu`.

proto-ruu exists so that a user producing software with coding agents does not
have to perform or reason about routine Git versioning operations for work whose
boundary can already be identified.

When work is presented within an identifiable WorkBoundary, proto-ruu owns the
routine Git versioning progression required to durably represent that work in
native Git and to complete its direct-push publication.

proto-ruu is deliberately bounded to publication that can be realized by a
direct Git push. Publication routes such as pull requests, merge queues,
stacked pull requests, provider-specific workflows, review-gated publication,
or general convergence are outside its current product domain; proto-ruu does
not invent a generic publication abstraction for them.

proto-ruu is a transitional component: it provides that bounded experience
while broader systems carrying richer publication and convergence
responsibilities are not yet available.

proto-ruu acts only within the supplied WorkBoundary. It does not discover,
claim, or govern unrelated work outside it.

Several proto-ruu invocations may progress concurrently. If the Git reality an
invocation depends on changes incompatibly while it progresses — for example
because another contribution advanced the same remote — proto-ruu does not
merge, rebase, reconcile, force-push, or otherwise absorb that concurrency. It
stops safely and returns a blocked result to the caller, who owns how its
objective continues.

The repository is currently in the product-definition phase.

Its authoritative starting point is:

- [`docs/specification/proto-ruu-spec.md`](docs/specification/proto-ruu-spec.md) — normative
  product meaning;
- [`docs/adr/`](docs/adr/) — accepted decision history;
- [`docs/vision/proto-ruu-vision.md`](docs/vision/proto-ruu-vision.md) — non-normative
  motivation and direction;
- [`docs/repository-governance/`](docs/repository-governance/) — repository
  procedure and engineering governance.

No implementation architecture, programming language, runtime, persistence
mechanism, orchestration runtime, locking strategy, commit-planning algorithm,
branch topology, worktree topology, remote provider, recovery mechanism, or
formal model is established merely by this repository layout.

No invariant identifiers have been admitted yet.

Implementation must be derived from accepted product semantics rather than
retroactively defining them.
