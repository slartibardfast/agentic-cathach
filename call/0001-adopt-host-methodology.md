# Adopt the host methodology and embed cathach as the Where room

- Status: accepted
- Scope: adoption
- Date: 2026-07-04

## Context and Problem Statement

`agentic-cathach` began as a bare repository holding only a `LICENSE`. It is meant
to be the host, the externalized thought, for the software `cathach`
(`github.com/slartibardfast/cathach`), which is itself a fresh repository. The host
needs governance, the rooms, the verification tooling, and a commit-time hygiene
gate, established the way the `host` process prescribes, so that a fresh session can
read the repository and continue the work.

## Decision

Adopt the methodology template at revision
`565410a860abab47b4782342374a1b9b2f08ddcb` (recorded in `.host`), as migration case
(a). Copy the canonical `CLAUDE.md` and `STRUCTURE.md` spine verbatim, scaffold
`cast/`, `plan/`, and `call/`, and record the project's own conventions under a
project-specifics heading. The recorded conventions are cross-platform Rust for all
software, specification-driven development, and a data-gathering phase ahead of the
Rust pipeline.

Embed `cathach` as the Where room: a `[software "cathach"]` stanza in
`.host-software` pinned at `617cb57630b06247d551a68562643f6fd54ca389`, materialized
as a bare store with a worktree under `software/cathach/`.

Install the `host-lint` commit hooks from the pinned `tools/host-lint` build rather
than from a `.host-software` gating component. A gating component embeds the tool as
software, which suits a host that authors the tool; `agentic-cathach` only adopts
it, so a gating component would duplicate the reference submodule against the
reference-do-not-vendor rule. The hooks and the binary are re-installed per clone,
the way git hooks always are.

Pin the verification tools at releases that satisfy the template's own upgrade
ledger. The commits its submodules reference are too old for that job: the adopted
revision's `UPGRADING.md` baseline requires host-lifecycle v0.32.0, and ledger rule
`a22704e` states the pin must sit at or above the ledger's maximum `requires`, yet the
template pins `tools/host-lifecycle` at v0.15.1, which has no receipt command and so
cannot run its own ledger. This host therefore pins `tools/host-lifecycle` at v0.35.1
and `tools/host-lint` at v0.12.1, both built from source under Rust 1.96.1.

## Consequences

- The requirements (`allium`) and timing (`specula`) lanes, and the
  reproducible-build recipe, stay inert until `cathach` grows a spec or a build. The
  project-specifics heading records when to wire each.
- Later upgrades diff from the recorded revision through the template's
  `UPGRADING.md` ledger (`host-lifecycle upgrade`).
- Because the gate is installed by hand rather than by `software --install-hooks`,
  no gating-component receipt is emitted for it, so the reasoning lives here instead.
- The lifecycle phase receipts live in `.host-receipts` (adopt, upgrade) and
  `.host-lifecycle-receipts` (classify, embed, remap, verify, publish, release);
  `host-lifecycle software --check` re-verifies each `done` by its manifest recheck.
