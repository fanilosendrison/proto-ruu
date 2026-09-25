---
okf_version: "1.0"
kind: "KnowledgeAsset"
asset_type: "agent-directives"
domain: "proto-ruu"
severity: "strict"
name: "proto-ruu repository agent directives"
---

# proto-ruu repository directives

Use this file as the operational map for the `proto-ruu` repository.

The repository is currently specification-first.

Do not infer implementation architecture from the repository name, from
proto-Go, from Ruu, or from adjacent projects.

## Product boundary

`proto-ruu` is currently defined by the Product Intent in:

```text
docs/specification/proto-ruu-spec.md
```

proto-ruu operates on a supplied WorkBoundary.

It durably represents the work contained by that boundary in native Git and
completes its direct-push publication. Publication routes requiring pull
requests, merge queues, provider-specific workflows, or general convergence
are outside its product domain.

It must not widen that authority merely because other repositories, linked
worktrees, checkouts, branches, sessions, or dirty working trees are
discoverable on the same machine.

The caller is responsible for supplying the work boundary it is authorized to
present. proto-ruu is responsible for respecting it.

Do not add invariants, canonical terms, architecture, or product semantics
beyond the accepted Product Intent without explicit semantic authority.

The `/ruu` invocation surface and any harness integration translate available
session and Git context into a WorkBoundary. Their mechanics are not proto-ruu
product semantics.

Do not add a product invariant stating that proto-ruu owns semantic validation,
commit policy, review policy, or publication policy merely because a previous
mechanism such as git-commits-push contained equivalent behavior.

## Authority by responsibility

1. `docs/specification/proto-ruu-spec.md`
   defines current normative product meaning.

2. Accepted ADRs under `docs/adr/`
   record explicit decisions and amendments.

3. `docs/vision/proto-ruu-vision.md`
   is non-normative.

4. `docs/repository-governance/`
   governs repository procedure only.

5. Future formal artifacts may check accepted semantics for their declared
   scope but do not replace normative Product Intent.

6. Future implementation and tests must conform to accepted authority and must
   not create missing product semantics.

Report inconsistencies between authoritative sources.

Do not silently choose the interpretation most convenient for implementation.

## Required reading

Before changing product semantics, deriving architecture, or preparing
implementation work, read:

1. `README.md`
2. `docs/specification/proto-ruu-spec.md`
3. `docs/adr/README.md`
4. `docs/adr/index.md`
5. `docs/repository-governance/proto-ruu-discovery-classification.md`
6. `docs/repository-governance/proto-ruu-engineering.md`

## Discovery handling

Every material discovery that affects product meaning must follow:

```text
docs/repository-governance/proto-ruu-discovery-classification.md
```

A missing semantic decision is not permission to improvise.

## Current implementation prohibition

At the current repository state, do not create:

```text
src/
bin/
tests/
formal/
qualification/
scripts/
package manifests
runtime configuration
database schemas
worktree registries
public APIs
```

unless a later explicit task is backed by sufficient accepted upstream
authority.

Do not select a programming language.

Do not select a persistence mechanism.

Do not select an orchestration runtime.

Do not select a locking strategy.

Do not select a commit-planning algorithm.

Do not select a branch topology.

Do not select a worktree topology.

Do not select a remote provider.

Do not select a recovery mechanism.

Do not select proto-Go, Ruu, or git-commits-push integration mechanics.

## ADR discipline

Do not create an ADR speculatively.

An ADR requires an actual identified decision.

A semantic ADR requires explicit product-owner resolution.

An architecture ADR requires sufficient upstream semantics to constrain the
decision.

## Repository naming

Use lowercase kebab-case for ordinary new files and directories except standard
entry points such as:

```text
AGENTS.md
README.md
```

The repository directory name `proto-ruu` does not make proto-Go, Ruu, or
git-commits-push semantic authority for this repository.

## Current target structure

The current authorized tree is:

```text
proto-ruu/
├── .gitignore
├── AGENTS.md
├── README.md
└── docs/
    ├── adr/
    │   ├── README.md
    │   ├── index.md
    │   ├── adr-001-concurrency-does-not-imply-convergence.md
    │   ├── adr-002-coordination-mechanism-undecided.md
    │   ├── adr-003-direct-push-publication-envelope.md
    │   ├── adr-004-transitional-target-stack.md
    │   └── adr-005-original-wording-correction.md
    ├── repository-governance/
    │   ├── proto-ruu-discovery-classification.md
    │   └── proto-ruu-engineering.md
    ├── specification/
    │   └── proto-ruu-spec.md
    └── vision/
        └── proto-ruu-vision.md
```

Do not expand this structure merely because a likely future directory can be
anticipated.
