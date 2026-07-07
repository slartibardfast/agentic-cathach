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

## 2026-07-05 — Prior-art grounding, the base-six problem, and a format proposal

- Added a sixth cast persona, Nuala the comparative linguist of number
  (`cast/nuala-numeral-linguist.md`, commit `5312b65`): a design advisor for the
  trilingual preamble's cross-linguistic soundness, not a decode-tier reader.
- Ran a five-thread prior-art survey (`plan/0001-durable-book-format/prior-art.md`,
  commit `dac734b`). Two load-bearing findings:
  - The hand-tier check pair (S = sum mod b, W = position-weighted sum mod b) is the
    RAID-6 dual-parity Reed-Solomon construction, a real distance-three code only over a
    field, i.e. a PRIME base. Base six is composite: the weighted guard silently loses
    jump-transposition detection, two-erasure recovery, and single-error correction, and
    a group caps at five data columns. Base seven (prime) restores all of it and allows
    six columns; base five is prime but caps at four. So base six was never settled and
    is a weak choice. Recommend base seven; if the alphabet measurement forces six,
    scope the guarantee to what the plain sum secures, or use a Verhoeff/Damm digit
    (loses hand-computability).
  - The preamble uses multiplication in W, but the assumed-knowledge floor (`call/0008`)
    is only counting, and no draft teaches multiplication. Fix: teach it as arrays before
    the guard. Alternative surfaced and verified: a Fletcher-style double running sum
    (A += digit; B += A) gives a transposition-catching weighted check with ADDITION ONLY
    over a prime base (weights are the reversed set, equal strength), so multiplication
    could be avoided entirely. Open fork.
- Advisor review (Fable 5) judged the milestone "inverted": effort is on the downstream
  preamble while the critical-path root, a measurement-protocol document with a decision
  table (measured rate -> radix/group/guards/interleave), does not exist and blocks three
  of four tasks. Honest-record defects it flagged: `poc-findings.md` mislabels a software
  simulation as `#measure-channel` results; print resolution contradicts (README locks
  300 DPI, `call/0004`/`architecture.md` say 600 dpi / 0.22 mm module).
- Drafted a format decisions-and-sequence proposal (`format-proposal.md`, status
  proposed, under operator review; browser render served at the poc gallery
  `localhost:8010/format-proposal.html`). It locks the decidable-now items and marks the
  measurement-gated ones, and names the decisions to record as `call/` entries once
  accepted: prime radix + guards; redundancy and erasure-mode decode; padding and the
  zero glyph; the integrity digests; preamble pedagogy plus a `call/0008` amendment
  (floor stays counting, arithmetic is taught not assumed); longevity and novelty
  positioning; finder reuse amending `call/0004`; and the DPI resolution.

## 2026-07-05 — Operator rulings and the hand-approved alphabet

- The operator ruled all six open questions in the format proposal: base 7; teach
  multiplication as arrays; a DISTINCT pad glyph (so the alphabet is eight glyphs, digits
  plus pad); two-tier distributed registration for finders (amends `call/0004`); lock
  600 dpi; digests per file and per book. Folded into `format-proposal.md` (commit
  `09ba43f`).
- Added a glyph HAND-APPROVAL gate: the eight glyph shapes need a human sign-off before
  adoption; measurement narrows candidates but does not decide.
- The operator then exercised that gate and HAND-APPROVED the alphabet: Hindu-Arabic
  digits 0 to 6 for the values, and a SALTIRE (diagonal cross) for the distinct pad. The
  payload and the preamble now share the digit alphabet. Measurement's remaining role is
  to VALIDATE the chosen shapes (watch the digit zero against six for confusion), not to
  choose them; a failing pair returns to the operator, not a silent change. The pad
  saltire contributes zero to both guards while being visually a non-digit.
- Served review artifacts (gitignored, in the poc gallery): the proposal rendered at
  `localhost:8010/format-proposal.html`, and an alphabet preview at
  `localhost:8010/alphabet-preview.html` (built via a small scratchpad md-to-html script,
  since no pandoc or python-markdown is installed). NOTE: those ad-hoc renders were removed
  in the refactor below; the doc site is now the generated mdBook.

## 2026-07-05 — Refactor to the methodology's mdBook doc-site framing

- Correction to the entry above: reviewing host docs by hand-rolling HTML (a scratch
  md-to-html script plus a python http server) and dropping host `plan/` renders into the
  SOFTWARE poc output dir violated the spine. STRUCTURE.md names `host-lifecycle book .` the
  canonical publisher (do not hand-roll a generator), and the rooms stay separate. Removed the
  host-doc HTML from `software/cathach/main/poc/out`.
- The doc site is the generated mdBook: `host-lifecycle book .` writes `book.toml` +
  `mdBook/src` + `SUMMARY.md` (all gitignored), `mdbook build` renders `mdBook/out`, and
  `mdbook serve` on :8010 is the review surface. The book surfaces one page per milestone (its
  README) plus cast, call, reference, memory. Supporting milestone docs (prior-art,
  measurement-protocol, format-proposal, architecture, preamble drafts) are linked from the
  milestone README, not separate book pages.
- Reconciled `plan/0001` drift: the README "Open items" restated the floor, radix, alphabet,
  and third language as open when call/0008 through call/0015 had decided them; replaced with a
  governing-decisions summary pointing to those decisions and naming what stays genuinely gated
  on measurement. Updated build-sequence task prose to cite the decisions and the measurement
  protocol (anchors and verify/depends fields unchanged). Relabelled `poc-findings.md` honestly
  as a simulation, not physical `#measure-channel` results. Marked `format-proposal.md` accepted,
  a frozen record whose decisions now live in call/0009 through call/0015.
- Gate stays green: `software --check` exit 0 (no HAZARDs, task graph intact), `book --check`
  ok, prose and reconcile clean. `publish` remains a valid skip (no Site CI workflow or Pages
  deployment yet); that is the one remaining step to publish done, and it is an outward-facing
  choice for the operator.

## 2026-07-05 — Doc site published; publish phase done

- The operator authorized publishing, so the reference Site workflow is wired and GitHub Pages
  is enabled. The doc site is live at https://slartibardfast.github.io/agentic-cathach/, serving
  the generated mdBook from the gh-pages branch. The Site workflow (on push to main) builds the
  host-lifecycle publisher from the pinned submodule, runs `book` + `book --check`, `mdbook
  build`, and deploys `mdBook/out` to gh-pages; Pages was enabled via `gh api ... /pages`.
- GOTCHA: the publish phase's manifest recheck is `test -f .github/workflows/mdbook.yml`, but the
  template ships the reference workflow as `.github/workflows/site.yml`. The workflow file MUST
  be named `mdbook.yml` or the publish=done recheck fails and re-opens as a HAZARD. Renamed the
  file to `mdbook.yml`; its content is the template's reference verbatim (name: Site inside).
- Recorded publish done with `host-lifecycle receipt --record publish --disposition done`;
  `software --check` is green with the phase done. The receipts ledger is append-only, so the
  earlier skip stays below the new done.

## 2026-07-05 — Reviewed for undone; reconciled drifted plan docs

- GOTCHA (binary provenance): a stray `/tmp/opencode/host-build/host-lifecycle` is an OLDER
  build that expects the pre-call/0039 layout (a bare repo named `.git`) and falsely HAZARDs
  `MISSING software/cathach/.git`. The current layout is call/0039's `.bare` store + a `.git`
  gitdir-link file, which the PINNED v0.36.0 tool wants. Always run the session's own built
  binary at `scratchpad/bin/host-lifecycle` (v0.36.0), not whatever `find /tmp` turns up. With
  the right binary the gate is exit 0.
- Reviewed for undone. Mechanical gate green, tree clean, nothing unpushed. Found doc-drift:
  live plan docs still asserted pre-decision values, in one case contradicting a decision that
  records itself as applied. call/0014's consequence says the README "is updated to reflect the
  600 dpi lock", but the README still locked 300 DPI in three places (incl. a Locked-constraints
  table). poc-findings' go/no-go gates and both preamble drafts still presented base six as
  current, which format-proposal.md:334 requires to "read as such" (as placeholders).
- Fix (commit `03f5c6e`): added one superseding note to each of the four docs, pointing at
  call/0009 (base seven + hand-approved eight-glyph alphabet) and call/0014 (600 dpi). Did NOT
  rewrite the imported research note, the simulation's own base-six output, or the draft bodies;
  the base-six worked rows stay as records, marked as placeholders to be revised under the
  pending #draft-preamble task. Gate stays green (prose clean, book renders, software --check
  exit 0).
- Still legitimately pending (not defects): the four milestone tasks (blocked on the operator +
  hardware for #measure-channel), and the inert spec/build lanes (wait on cathach gaining code).

## 2026-07-05 — Settled the measurement protocol's design parameters

- The operator ruled the design-side open parameters of measurement-protocol.md (section 7 now
  "Design parameters, set"; section 8 holds the two hardware ones for the bench). Rulings:
  - Module sizes: four candidate modules at 600 dpi on clean dot counts, 0.17 mm (4 dots),
    0.21 mm (5, the ~0.22 mm target), 0.25 mm (6), 0.34 mm (8, legibility reference).
    Instructional register 3.0 mm cap-height, tested to the 2.8 mm floor.
  - Accept thresholds (keying the section-5 decision table): silent-substitution bands at 0.1%
    and 1% per glyph; located-erasure boundary at 90%; visual-distance floor 99.9% correct pair
    discrimination undamaged; the 0-vs-6 pair is the acceptance gate.
  - Ambition: FULL run up front (0.1% resolution). ~500 reads per glyph x module size x damage
    mode at a fixed dose (~12,000 reads/glyph aggregate), a 3-dose severity sweep per mode at the
    0.21 mm target, ~3,000 undamaged reads per glyph per size for the discrimination baseline,
    and ~30 damaged specimens for the two-tier registration battery.
  - Phone-capture bar: convenience-only. A flat-page phone photo in good light must register the
    two-tier finders and decode the coarsest 0.34 mm module; no full dense decode off a phone is
    required (flatbed primary, call/0014).
- Recorded in the protocol itself, not a new call/ (these refine call/0009/0010/0014; the operator
  can promote them to a decision later if wanted). Updated undone-review.md to match. Gate green.
- STILL OPEN before #measure-channel runs: the two hardware parameters (ISO 9706 paper stock;
  printer + monochrome toner), then the operator's approve-before-printing gate (the protocol's
  gates section).
- GOTCHA (host-lint allowlist is inert): the pre-commit hook invokes host-lint as
  `git show ":$file" | host-lint --stdin-as "$file"` with NO allowlist flag, and the binary does
  not auto-discover `.host-lint-allow` (a declared `2.8 mm` still warns). So `.host-lint-allow` is
  currently DOCUMENTARY only, not consulted. Measurement decimals (0.17/0.21/2.8 mm, 0.1/99.9
  percent, etc.) therefore emit advisory `warning:` lines (host-lint rc 3), which the hook prints
  but does NOT block on (only rc 1 = confirmed tell blocks; rc 0/3 pass). software --check does not
  run the naming audit, so the gate stays green regardless. Do not cargo-cult entries into the
  inert allow file. Positional "section N" prose cross-refs DO warn and are a real tell-shape; I
  reworded mine to directional content ("fixed below", "the decision table above") to match the
  protocol's own established style ("listed at the end", "the decision table below").

## 2026-07-07: Cast humanist review, recorded and addressed

- Ran the six-persona humanist review of plan/0001 at the operator's request; findings and
  dispositions live in plan/0001-durable-book-format/humanist-review.md.
- The sharp findings: (a) subtraction, the repair's own operation, was used but never taught
  and absent from the budget — the same taught-never-assumed breach call/0013 closed for
  multiplication. Closed by call/0016 (Amended lines added to call/0008 and call/0013).
  (b) The blind hand-decode trial that format-proposal.md step-sequence said "should be added
  to the milestone's build sequence as a task" had never been added; now #blind-decode-trial,
  depends #draft-preamble, with a figure-only pass (reader works from figures with prose in a
  language they cannot read) that de-risks call/0007 over call/0015's horizon.
- Protocol amendments (pre-approval, so no re-approval churn): a source-transcription specimen
  (decoder source character set through the damage battery, for Fintan's tier) and a
  readers-and-conditions rule (eye reads record age + lighting; naked-eye floor judged against
  an older unaided eye — the far-future reader has no optometry).
- Task-text amendments: #draft-preamble gains the trilingual equivalence review gate (Nuala)
  and call/0016; #scope-rust-pipeline gains the printed known-answer-test spec obligation
  (Fintan) and records Aoife's browser-decode path as owed its own milestone (it existed in
  no plan at all — her modality was unserved beyond the phone-capture bar).
- CLAUDE.md project-specifics said "five personas"; the cast holds six (Nuala joined later).
  Corrected.
- Gate green end to end: validate ok, prose clean, reconcile clean, tasks parse (5 tasks,
  frontier unchanged at #measure-channel), book renders (call room now 17 pages), software
  --check exit 0. All commits pushed individually per the audited-plans discipline.
