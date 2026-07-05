# Undone review: what remains before the measurement gate

- Status: review, current
- Scope: cathach
- Date: 2026-07-05
- Serves: [Bríd](../../cast/brid-archivist.md) (primary), and the wider cast

A snapshot of the milestone's state: what is settled and what genuinely remains
undone. It is a review record, not a decision. The decisions it refers to govern;
where this snapshot and a `call/` entry ever disagree, the decision wins.

## The mechanical gate is green

`host-lifecycle software --check` exits zero. Every lifecycle phase carries a receipt,
and `publish — done` covers the live doc site. The components sit at their recorded
pin, the upgrade ledger is clean, and the prose and reconcile lanes pass and the book
renders. The four milestone tasks are pending with no receipt, which is correct while
the milestone is open.

One trap is worth naming for a future session. A stray older `host-lifecycle` binary
left under a scratch path predates the tool's `.bare` store layout and will falsely
report `MISSING software/cathach/.git`. The pinned
v0.36.0 tool wants the current `.bare` store beside a `.git` gitdir-link file, which is
what the tree already holds. Run the pinned binary, and the gate reads clean.

## Drift that was reconciled

Three live documents had fallen behind the recorded decisions. One case contradicted a
decision that records itself as already applied. All three were corrected with a single
superseding note each. None rewrote the imported note or the simulation output, and
none touched the draft bodies.

| Document | Drift | Correction |
|---|---|---|
| [README.md](README.md) | Locked 300 DPI in three places, though [call/0014](../../call/0014-print-resolution.md) records the README as updated to 600 dpi | A note marks the reproduced note's physical figures historical and points at the governing decisions |
| [poc-findings.md](poc-findings.md) | Go/no-go gates presented the base and alphabet as open questions | A note closes those gates against [call/0009](../../call/0009-payload-symbols-and-guards.md) |
| [preamble-draft.md](preamble-draft.md), [preamble-proposal-nuala.md](preamble-proposal-nuala.md) | Taught base six as current fact, which [format-proposal.md](format-proposal.md) requires to read as a placeholder | A placeholder banner ties the revision to the pending draft task |

The base-six worked rows stay as records of the simulation and the early drafts. They
are now marked as placeholders to be revised when the preamble is drafted against the
settled base.

## What genuinely remains undone

These are not defects. They are work that waits on an input the project does not yet
have.

- **The physical measurement.** The critical-path root, `#measure-channel`, waits on the
  operator. The protocol is in [measurement-protocol.md](measurement-protocol.md), and its
  design parameters are now set (module sizes, accept thresholds, sample counts, and the
  phone bar, in section 7). Two hardware parameters stay open for the bench, the paper stock
  and the printer with its toner, and the operator approves the protocol before printing and
  then runs the print-damage-scan. Three of the four milestone tasks depend on it.
- **The spec and build lanes.** The `.allium` behaviour spec, any `.tla` timing spec,
  and the reproducible-build recipe stay inert until `cathach` gains code. That code is
  scoped only after the measurement settles the format, so the lanes wait by design.

## Bottom line

The project is consistent with its own recorded decisions. The path forward is
unchanged and belongs to the operator: set the measurement parameters, then run
`#measure-channel`. Everything downstream of that gate is already sequenced.
