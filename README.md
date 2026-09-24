# proto-ruu

`proto-ruu` is the routine Git versioning boundary operating over a supplied
WorkBoundary.

The user-facing invocation of proto-ruu is `/ruu`.

proto-ruu exists so that a user producing software with coding agents does not
have to perform or reason about routine Git versioning operations for work whose
boundary can already be identified.

When work is presented within an identifiable WorkBoundary, proto-ruu owns the
routine Git versioning progression required to durably represent that work and
advance it across the authorized Git boundary, including publication when
publication is part of the invocation contract.

proto-ruu acts only within the supplied WorkBoundary. It does not discover,
claim, or govern unrelated work outside it.

The repository is currently in the product-definition phase.

Its authoritative starting point is:

- [`docs/specification/proto-ruu-spec.md`](docs/specification/proto-ruu-spec.md) — normative
  product meaning;
- [`docs/adr/`](docs/adr/) — accepted decision history once product decisions
  are recorded;
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
