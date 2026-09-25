---
okf_version: "1.0"
adr_profile_version: "0.1.0"
kind: "KnowledgeAsset"
asset_type: "architecture-decision-record"
domain: "proto-ruu"
severity: "strict"
name: "proto-Ruu's validity envelope is bounded to direct-push publication"
id: "ADR-003"
status: "accepted"
date: "2026-09-25"
decision_body_sha256: "3bb947adf6f91b1ad36eb6fb7427ae85cf125f8c1755e438e8005fa899facf02"
relation_completeness: "complete"
relations:
  clarifies: []
  amends: []
  supersedes: []
  confirms: ["ADR-001"]
governs:
  - "proto-ruu publication validity envelope"
  - "direct-push publication as the normal supported outcome"
  - "exclusion of pull requests, merge queues, provider-specific workflows, and convergence from the proto-ruu domain"
  - "proto-ruu's transitional, non-Ruu-evolutionary identity"
---

# ADR-003: proto-Ruu's validity envelope is bounded to direct-push publication

## Context

The Product Intent described publication as required "when publication is part
of the invocation contract" and referred to advancing work "across the
authorized Git boundary" or to "authorized publication when applicable." That
formulation admits an abstract family of publication routes — direct push,
pull requests, merge queues, provider-specific workflows, review-gated
publication — without bounding the product. It also left the expected terminal
outcome of an invocation ambiguous.

The product owner has resolved the publication scope. proto-Ruu provides the
"do not worry about Git" experience for already identified work only in the
sub-domain where the required publication can be realized by direct Git push.
proto-Ruu is a transitional component used while Turnlock, proto-Go, and Ruu
are not available. It is not intended to become Ruu progressively or to
generalize Ruu's publication model.

## Decision

proto-Ruu is deliberately bounded to the direct-push publication domain.

Inside the supported domain, the normal expected outcome of an invocation is
that the supplied work is durably represented in native Git AND its required
direct-push publication has been completed. Direct push is not one option
inside a general family of publication routes; it is the publication
realization proto-Ruu supports in this generation.

Work whose correct progression requires another publication mechanism is
outside the current proto-Ruu product domain, including:

- pull requests;
- merge queues;
- stacked pull requests;
- provider-specific publication workflows;
- review-gated publication;
- general convergence;
- publication topology selection;
- multiple publication route families.

proto-Ruu MUST NOT respond to such work by inventing an unsupported publication
mechanism, a generic publication abstraction, or a route engine. The case is
out of scope, not partially supported.

The bounded direct-push domain does not relax concurrency handling: force-push,
automatic merge, automatic rebase, and automatic convergence remain prohibited
as ways to make publication succeed. ADR-001 and ADR-002 remain applicable;
this record confirms ADR-001.

proto-Ruu is a temporary bounded solution. It is not an incremental version
destined to become Ruu, and it MUST NOT progressively acquire Ruu's broader
publication, provider, convergence, or managed-state responsibilities.

Conceptually, a presented case falls into one of three semantic classes:

```text
SUCCESS
    the work is inside the supported domain, and its durable Git
    representation and direct-push publication are complete

BLOCKED
    the work is inside the supported domain, but this invocation can no longer
    progress safely under the authority and Git preconditions it holds

OUT OF SCOPE
    the required publication does not belong to proto-Ruu's validity envelope
```

These classes are conceptual. Enum names, exit codes, APIs, result schemas,
implementation-level error taxonomy, and CLI wording are not decided by this
record.

The following also remain undecided: remote naming; branch or ref inference;
the exact push protocol; force-with-lease or compare-and-swap mechanisms;
credential handling; retry representation; and output schema.

## Alternatives considered

- **Keep "publication when applicable" and support several publication
  routes:** rejected because it would turn proto-Ruu into a general publication
  engine and grow it toward Ruu by accident.
- **Treat pull-request or merge-queue cases as future extensions to
  "prepare":** rejected because proto-Ruu's simplicity is intentional and must
  not import a future Ruu abstraction.
- **Allow concurrency handling to be relaxed so direct push always succeeds
  (force-push, merge, rebase):** rejected because ADR-001 and ADR-002 prohibit
  convergence and the direct-push envelope does not relax them.

## Consequences

### Benefits

- The product promise becomes decidable: success means durable Git
  representation plus completed direct-push publication.
- Out-of-scope cases are declared as such instead of pushing proto-Ruu toward a
  generic route abstraction.
- The boundary between proto-Ruu (transitional, bounded, direct push) and Ruu
  (durable target, richer publication and convergence) is explicit.

### Costs and obligations

- Repositories whose publication requires pull requests, merge queues, or
  provider-specific workflows cannot use proto-Ruu for those flows.
- Product Intent passages written around a generic publication contract must be
  rewritten to the bounded model.
- The exact direct-push mechanics remain undecided: remote naming, ref
  inference, push protocol, compare-and-swap or force-with-lease, credentials,
  retry representation, and output schema.

## References

- ADR-001: proto-Ruu does not resolve concurrency between contributions.
- ADR-002: The concurrency coordination mechanism remains undecided.
- `docs/specification/proto-ruu-spec.md`
- `README.md`
- `docs/vision/proto-ruu-vision.md`
