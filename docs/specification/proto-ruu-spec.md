# proto-Ruu — Requirements, Invariants, and Architectural Implications

> Working product specification derived from the current product discussion.
>
> This document intentionally starts from product intent before deriving
> invariants or selecting implementation mechanisms. Terms such as process
> model, persistence mechanism, orchestration runtime, locking strategy,
> commit-planning algorithm, branch topology, worktree topology, remote
> provider, or recovery mechanism are deliberately left unspecified unless
> required by the product contract.

# 0. Product intent — governing user experience

This section is normative for the initial proto-Ruu product direction.

It states the user-visible outcome that later invariants and architecture exist
to serve.

A technical convenience is not sufficient reason to weaken this product
promise. If later derivation shows that part of this promise cannot be
implemented safely within proto-Ruu's deliberately bounded scope, that
limitation must be made explicit rather than introduced accidentally by the
implementation.

## 0.1 Product definition: invisible Git versioning over supplied work

`proto-ruu` exists so that a user producing software with coding agents does
not have to perform or reason about routine Git versioning operations for work
whose boundary can already be identified.

The user-level objective is:

> **I do the work; proto-Ruu deals with versioning it.**

Git remains the versioning substrate and interoperability boundary.
proto-Ruu does not replace Git with another version-control system.

What proto-Ruu removes from the ordinary user workflow is the need to manually
decide and execute routine operations required to turn identified working state
into the appropriate durable Git state and, when required by the invocation,
publish that state through Git.

The ordinary experience should therefore approach:

```text
implement
implement
implement

/ruu

continue working
```

rather than:

```text
inspect status
decide what to stage
stage files
decide commit boundaries
write commit messages
commit
inspect branch state
determine what remains unpublished
choose the appropriate push operation
push
verify publication
```

Those mechanics are product responsibility to the extent that they can be
determined safely from the supplied work boundary, Git state, and applicable
invocation authority.

proto-Ruu is not defined as a `git commit && git push` wrapper.

Its product responsibility is the disappearance of routine version-control
work from the user's cognitive workflow.

## 0.2 The product promise

proto-Ruu operates on a **WorkBoundary**.

A WorkBoundary identifies the Git working state that one proto-Ruu invocation is
authorized and expected to version.

Given such a boundary, the governing promise is:

> **When work is presented to proto-Ruu within an identifiable WorkBoundary,
> proto-Ruu owns the routine Git versioning progression required to durably
> represent that work and advance it across the authorized Git boundary,
> including publication when publication is part of the invocation contract.
> The user or calling system does not need to manually perform or reconstruct
> the corresponding staging, commit, history, or publication operations.
> proto-Ruu acts only within the supplied WorkBoundary and does not discover,
> claim, or govern unrelated work outside it.**

This is the defining reduction relative to Ruu.

proto-Ruu does not promise to discover the complete universe of work that ought
to participate in an invocation.

It promises to correctly version the work universe that has been presented to
it.

Conceptually:

```text
WorkBoundary
     ↓
 proto-Ruu
     ↓
durable / advanced Git state
```

not:

```text
entire development environment
     ↓
proto-Ruu discovers all relevant work
     ↓
proto-Ruu globally coordinates everything
```

## 0.3 WorkBoundary is authority, not global discovery

The WorkBoundary is the semantic limit of an invocation.

It may identify work in one Git workspace or in several Git workspaces across
one or more repositories.

For example:

```text
WorkBoundary
  ├─ repository A / workspace X
  ├─ repository B / workspace Y
  └─ repository C / workspace Z
```

proto-Ruu may inspect and version all applicable state inside those supplied
surfaces.

It MUST NOT widen that authority merely because other repositories, linked
worktrees, checkouts, branches, sessions, or dirty working trees are
discoverable on the same machine.

Therefore:

```text
all changes inside the supplied WorkBoundary
                       !=
all changes in all discoverable Git workspaces
```

The presence of additional work elsewhere is not an invitation to adopt,
checkpoint, publish, reconcile, or otherwise mutate it.

The caller is responsible for supplying the work boundary it is authorized to
present.

proto-Ruu is responsible for respecting it.

## 0.4 proto-Go is a first-class WorkBoundary provider

proto-Go is an intended first-class caller of proto-Ruu.

When proto-Go manages a contribution, proto-Go already owns higher-level facts
such as:

* which logical contribution is progressing;
* which repositories participate in that contribution;
* which managed authoring surfaces belong to it;
* when authored work has reached the applicable handoff boundary;
* which publication obligations belong to that handoff;
* when aggregate proto-Go publication requirements are satisfied.

proto-Go can therefore present proto-Ruu with a precise WorkBoundary and the
authority required to perform the corresponding Git versioning progression.

Conceptually:

```text
proto-Go
   │
   │ exact work boundary + applicable Git authority
   ▼
proto-Ruu
   │
   │ versioning effects / outcomes
   ▼
Git
```

proto-Ruu MUST NOT require proto-Go to rediscover or manually reproduce routine
Git mechanics that proto-Ruu can own itself.

Conversely, proto-Ruu MUST NOT absorb proto-Go's semantic ownership of the
ManagedContribution, readiness, validation, or aggregate publication lifecycle.

If one proto-Go handoff spans several repositories, proto-Go remains the
authority that decides which repository-local obligations belong to the same
logical contribution and when their aggregate completion establishes the
higher-level proto-Go outcome.

proto-Ruu realizes Git effects inside the supplied boundary; it does not redefine
proto-Go's lifecycle.

## 0.5 proto-Ruu must also be usable without proto-Go

proto-Ruu is not merely an internal proto-Go implementation component.

A user must be able to invoke it from an ordinary coding-agent session even when
that work was not created under proto-Go.

The intended standalone interaction is skill-like:

```text
user is already working in a coding-agent session
  → work has been produced in an ordinary Git environment
  → user invokes /ruu
  → the invocation adapter establishes the strongest WorkBoundary it can
    authoritatively identify
  → proto-Ruu versions that boundary
```

The ordinary standalone experience MUST NOT require the user to construct an
internal WorkBoundary object manually.

The invocation surface or harness integration is responsible for translating
the available session/Git context into a WorkBoundary.

When no richer authoritative context exists, the current Git workspace may form
the available work boundary.

That limitation must remain visible in the semantics:

```text
current observable workspace
!=
proven causal set of changes authored by this session
```

proto-Ruu MUST NOT fabricate provenance it does not possess.

A standalone invocation may therefore have a less precise WorkBoundary than a
proto-Go invocation while using the same proto-Ruu core semantics.

## 0.6 A dedicated Git worktree is not a proto-Ruu prerequisite

proto-Go may choose to provide dedicated temporary Git worktrees because its own
managed-authoring semantics require them.

That does not make dedicated linked worktrees a proto-Ruu product requirement.

proto-Ruu must be able to operate on an ordinary Git checkout when that checkout
is the supplied WorkBoundary.

Therefore the product distinction is not:

```text
worktree
vs
no worktree
```

but:

```text
identifiable authorized Git workspace
vs
no sufficiently identifiable work boundary
```

A normal primary checkout, a linked worktree, or another conforming Git
workspace may participate when it can be identified and safely operated on.

proto-Ruu MUST NOT require that the work was originally produced under
proto-Go.

## 0.7 Work ownership is supplied, not inferred globally

proto-Ruu is deliberately not responsible for reconstructing causal authorship
from arbitrary ambient filesystem state.

Suppose an ordinary checkout already contains unrelated modifications and a new
session adds additional modifications.

Git alone may expose only:

```text
modified A
modified B
modified C
```

without enough information to establish:

```text
A = older unrelated work
B,C = work from this session
```

proto-Ruu MUST NOT invent that distinction.

The product rule is:

> **proto-Ruu versions the work contained by the supplied WorkBoundary; it does
> not infer work ownership beyond the authority and evidence supplied by the
> invocation context.**

A richer caller such as proto-Go may provide a precise boundary.

A standalone invocation may provide only the current workspace.

Future harness integrations may provide richer session-scoped evidence without
changing proto-Ruu's core responsibility.

## 0.8 proto-Ruu owns versioning, not semantic validation

proto-Ruu exists to make versioning disappear, not to become the authority on
whether authored work is good, complete, reviewed, tested, or semantically
correct.

It does not inherently own decisions such as:

```text
is the feature complete?
are these tests sufficient?
has review been satisfied?
is the implementation correct?
should this work be published from a product perspective?
which repositories semantically belong to the same task?
```

Those decisions belong to the caller, Development System, repository policy,
authorized human, or another applicable authority.

For proto-Go specifically, applicable validation obligations are governed by
proto-Go's own product semantics before the corresponding handoff.

proto-Ruu may enforce checks that are necessary for the correctness or safety of
its own Git effects, but such checks must not silently become independent
product-quality policy.

Therefore a future implementation must not inherit tests, secret-scanning
policy, Conventional Commit policy, review policy, or similar GCP behavior
merely because git-commits-push previously contained those mechanisms.

Each such capability must be justified independently by proto-Ruu's own product
contract or by explicitly supplied external policy.

## 0.9 Commit structure and Git mechanics are implementation responsibilities, not user workflow

The user should not ordinarily need to decide the mechanical representation of
the versioning progression.

proto-Ruu may need to determine or obtain sufficient authority for matters such
as:

```text
what must be staged
whether new commits are required
whether already-existing commits satisfy part of the work
how commit metadata is produced
whether multiple commits are necessary
what exact Git object is to be published
whether publication has actually occurred
how a retry avoids duplicating already-realized effects
```

The exact mechanisms are not established by this Product Intent.

In particular, this document does not establish:

* Conventional Commits as mandatory;
* one commit per file;
* one commit per invocation;
* LLM-generated commit messages;
* a particular branching model;
* a particular remote name;
* direct push versus pull request;
* a specific orchestration runtime;
* a SQLite reconciler;
* a particular retry or persistence mechanism.

Those are downstream decisions.

The governing criterion is whether the resulting behavior preserves the
user-facing promise that routine Git versioning has ceased to be the user's
responsibility.

## 0.10 Existing Git state is part of the problem, not necessarily an error

The supplied WorkBoundary may contain different legitimate Git states.

For example, work may be:

```text
dirty and not yet committed
already committed but not yet published
partially versioned
clean because the required Git state already exists
```

proto-Ruu must reason from actual Git state rather than assuming that every
invocation begins with a dirty working tree requiring a fresh commit.

The product outcome is successful versioning progression, not unconditional
execution of a fixed sequence of Git commands.

Therefore:

```text
proto-Ruu
!=
always git add → git commit → git push
```

A conforming implementation must recognize when an effect already exists and
must not recreate it merely to satisfy a procedural script.

## 0.11 Retries must not turn invisible versioning into duplicate versioning

The ordinary user experience assumes that invoking proto-Ruu, retrying after an
interruption, or being retried by a calling workflow does not require the user
to manually reconstruct which Git effects already occurred.

Therefore proto-Ruu's eventual architecture must preserve enough exact evidence
to distinguish:

```text
effect not yet realized
effect already realized
effect realized locally but not yet published
publication already realized
state changed such that the requested effect can no longer be safely continued
```

This Product Intent does not yet choose the persistence or recovery mechanism.

It does establish the product requirement that crash/retry handling must not
depend on blindly replaying mutating Git operations and hoping they remain
harmless.

## 0.12 Native Git remains the observable substrate

proto-Ruu's effects must remain represented as ordinary Git state.

A user, agent, or external tool must be able to inspect the resulting commits,
history, refs, and remote state using ordinary Git mechanisms.

proto-Ruu may maintain additional internal evidence when required for safe
operation, but that evidence must not replace native Git history as the
representation of the versioned work itself.

The product dependency direction is:

```text
supplied work
    ↓
proto-Ruu versioning semantics
    ↓
native Git state/history
    ↓
authorized publication when applicable
```

## 0.13 Concurrent progression may block; proto-Ruu does not converge

A proto-Ruu invocation progresses the WorkBoundary it was supplied. Several
proto-Ruu invocations may therefore exist and progress at the same time.

Independent WorkBoundaries do not require global serialization merely because
they coexist. The product property is not:

```text
only one proto-Ruu invocation may exist at a time
```

but:

```text
independent proto-Ruu invocations may progress concurrently

if one invocation's required Git preconditions are invalidated by another:
    block safely
    return control to the caller
```

An invocation acts only under the authority and Git assumptions applicable to
its own WorkBoundary. It must not widen that authority because other
contributions, workspaces, invocations, or Git realities are discoverable.

If, during progression, a Git reality required by the operation changes in a
way that makes the requested effect no longer demonstrably safe under the
authority the invocation holds — for example because another contribution has
advanced the same remote destination — proto-Ruu MUST NOT:

- merge;
- rebase;
- resolve conflicts;
- force-push;
- adopt another contribution;
- otherwise absorb the concurrency;
- perform general convergence.

It MUST stop that progression safely and return to the caller a result
indicating that progression is blocked by the observed concurrent Git reality.

That blocked outcome is an observable truth of the invocation, not a failure
proto-Ruu is responsible for repairing.

proto-Ruu MAY detect concurrent change. Detecting concurrent change MUST NOT
become general convergence ownership.

What happens after the block belongs to the caller / Development System. A
proto-Go caller may, for example, preserve the same ManagedContribution
objective, retire prior readiness authority if authored mutation must resume,
request authored correction or convergence, revalidate affected work, establish
a later readiness occurrence, and invoke proto-Ruu again. proto-Ruu MUST NOT
know or own that caller-side continuation logic.

The exact blocked-result shape, API enum or schema, locking mechanism,
compare-and-swap versus lease or another Git protocol, lock granularity, retry
strategy, Git precondition representation, collision-detection algorithm, and
persistence model remain undecided.

This is an explicit difference between proto-Ruu and Ruu:

```text
proto-Ruu
= safe Git progression of supplied work
= concurrency encountered during that progression may block it

Ruu
= broader convergence system
= may mechanically reconcile concurrent managed work
```

proto-Ruu only detects concurrency encountered during safe progression of the
work it was given, and blocks instead of absorbing it.

## 0.14 proto-Ruu is deliberately not Ruu

proto-Ruu aims to reproduce a bounded form of the Ruu user experience:

> **the user should not have to think about version control.**

It deliberately does not yet reproduce Ruu's complete coordination model.

Ruu's stronger product problem includes discovering and re-observing durable
managed state, coordinating concurrent sessions and repositories, reconciling
stale lineages, maintaining managed authoring topology, handling global
convergence obligations, and progressing a distributed version-control system
without relying on a caller to supply the complete work boundary.

proto-Ruu instead assumes:

```text
someone can tell me which work I own for this invocation
```

and promises:

```text
once you tell me that, I make its routine Git versioning your problem no longer
```

This distinction is intentional.

proto-Ruu MUST NOT grow global work discovery, global work ownership inference,
or unrelated-work reconciliation merely to approximate Ruu incrementally.

If such capabilities become required, they belong to Ruu or require an explicit
change to proto-Ruu's Product Intent.

The architectural boundary is therefore:

```text
                  supplies WorkBoundary
                         │
        ┌────────────────┴────────────────┐
        │                                 │
    proto-Go                         /ruu adapter
 precise managed                    best available
   boundary                           boundary
        │                                 │
        └────────────────┬────────────────┘
                         ▼
                     proto-Ruu
                         │
                         ▼
                        Git
```

## 0.15 Governing product test

When evaluating a future invariant, architecture, implementation mechanism, or
migration from git-commits-push, the primary product question is:

> **Given work whose boundary has already been established, can the user or
> caller hand that work to proto-Ruu and stop thinking about its routine Git
> versioning?**

A mechanism that improves implementation convenience but forces the user back
into routine staging, commit management, publication recovery, or Git
coordination inside the supplied WorkBoundary works against the product intent.

A mechanism that attempts to solve unrelated work discovery or global
convergence outside that boundary may be useful to Ruu, but is outside
proto-Ruu unless separately admitted by an explicit product decision.
