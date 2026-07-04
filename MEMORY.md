# MEMORY

Append-only working memory. Newest entries at the bottom. Never rewrite or delete an
old entry; if one was wrong, add a new one that corrects it and points back. This
file is excluded from the naming and prose audits (`.host-lintignore`).

## 2026-07-04 — Adopted the host methodology (case a)

- `agentic-cathach` is the host for the software `cathach`
  (`github.com/slartibardfast/cathach`). Both repos started empty.
- Adopted template `github.com/connollydavid/host-template` at revision
  `565410a860abab47b4782342374a1b9b2f08ddcb`. Recorded in `.host`. The stamp's
  `adopted` date reads `2026-07-03` because `host-lifecycle adopt` took it from the
  build host's clock; the actual adoption day is 2026-07-04. The stamp is never
  hand-edited, so it stands as written.
- Case (a): copied `CLAUDE.md` + `STRUCTURE.md` verbatim, scaffolded `cast/ plan/
  call/`, added a project-specifics heading to `CLAUDE.md`. See `call/0001`.
- Project conventions (from the operator): all software is cross-platform Rust;
  development is specification-driven; a data-gathering phase precedes the Rust
  pipeline.
- Imported the cathach design note into `plan/0001-durable-book-format/`. It is a
  durable printed-book data encoding; the near-term work is measuring the
  print-and-damage channel to fix the error-correction parameters and glyph alphabet
  before any code. The measured milestone tasks carry `attested operator` verifies.
- `.host-lint-allow` declares the measurements `2.8 mm` / `0.34 mm` as quantities,
  not milestone codes, so the naming audit stays clean.

### Tooling

- Built `host-lint` (pinned `2ef53995855e4ec363ba5b587b176d49b9aad7a5`) and
  `host-lifecycle` (pinned `2a24deb0e5bcb3b3c09f50c39d7cfb84c445eafa`) from source
  under Rust 1.96.1 (the template's reproducible anchor is 1.95.0; the minor bump
  builds cleanly). The build tree lives in the session scratchpad, not in the repo.
- The `cathach` software pin is `617cb57630b06247d551a68562643f6fd54ca389` (its
  `main` HEAD), to record in `.host-software`.

### Pending operator approval (auto-mode trust gates)

These steps cross trust boundaries the harness would not auto-approve, so they are
not done yet:

- Wire the tool submodules (`host-template`, `tools/host-lint`,
  `tools/host-lifecycle`, `tools/allium`, `tools/specula`) at their pinned commits,
  then generate skills with `link-skills.sh`. `allium`/`specula` are inert until a
  spec exists and may be deferred.
- Write `.host-software` for `cathach` and run `host-lifecycle software
  --materialize .` to realise `software/cathach/`.
- Install the commit hooks (`pre-commit`, `commit-msg`, and the `host-lint` binary
  into `.git/hooks`).

Pinned commits for when these run: host-lint `2ef5399`, host-lifecycle `2a24deb`,
allium `82da292`, specula `38e9d6e`, host-template `565410a`.

## 2026-07-04 — Completed the external wiring; corrected the tool pins

The operator approved the external steps, so all of the pending items above are now
done. Two corrections to the entry above:

- Tool pins. The template pins `tools/host-lint` at v0.2.0 and `tools/host-lifecycle`
  at v0.15.1, but its own `UPGRADING.md` baseline requires host-lifecycle v0.32.0
  (ledger rule `a22704e`: pin at or above the ledger's maximum `requires`). The v0.15.1
  binary has no `receipt` command, so `software --check` HAZARDed the receipts entry
  `ac32d1c`. Rebuilt and re-pinned `tools/host-lifecycle` to v0.35.1 (`486add7`) and
  `tools/host-lint` to v0.12.1 (`78804cd`). See `call/0001`.
- Where-room layout. The v0.35.1 `.host-software` `worktrees` key takes branch names,
  not paths, and the canonical worktree comes from `branch` (default `main`). The
  recipe now records `branch = main` with no redundant `worktrees` line, materialized
  as `software/cathach/.git` (bare) plus `software/cathach/main`.

Recorded a receipt for all eight lifecycle phases: classify/adopt/embed/upgrade/verify
done, remap/publish/release skip with reasons. `host-lifecycle software --check` is
green (components at pin, upgrade ledger clean, prose clean, reconcile clean, every
phase receipted). The commit-hook tell test blocks a `Phase 1` message.

Still open (deliberately deferred, not blocking the gate): the doc-site CI lane
(`.github/workflows/mdbook.yml`, so `publish` can move from skip to done) and cathach's
reproducible-build recipe and specs, which wait on the software gaining code.
