# Payload symbols and the hand-computable guards

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

The payload is written as digit groups in some base, each group carrying two
hand-computable check digits: S, the plain sum, and W, the position-weighted sum, both
mod the base. The base, the guard count, the alphabet, and the padding convention were all
open. The base is not a free choice, because S and W are the RAID-6 dual-parity
Reed-Solomon construction, and that construction is a genuine distance-three code only over
a field, which for these moduli means a prime base (prior-art.md, "The verdict on the S and
W scheme and the base").

## Decision

The payload base is seven, a prime. The evidence is in prior-art.md and the analysis in
format-proposal.md:

- Composite base six forfeits what the weighted guard exists to add: detection of jump
  transpositions across columns two, three, or four apart, recovery of two located
  erasures, and correction of one unlocated error. It also aliases column weights, so a
  group caps at five data columns. What survives base six is only what the plain sum
  secures.
- Base seven restores every forfeited power, makes S and W a genuine distance-three code,
  and allows six data columns per group.

Both guards are kept. An unweighted sum catches no transposition, because a sum does not
depend on order. The guard count is the number of located erasures to recover per group
plus one, so two guards is correct when damage erases at most one glyph per interleaved
group. The hand-tier guarantee is scoped to what subtraction alone delivers: one located
erasure recovered, one substitution detected, adjacent transpositions detected. Anything
needing a modular inverse belongs to the scanner and rebuilt-tool tiers.

The alphabet is hand-approved by the operator. Values zero to six are the Hindu-Arabic
digits 0 1 2 3 4 5 6. The distinct end-of-data pad is a saltire, a diagonal cross drawn as
the two diagonals of the cell, chosen to read as neither a digit nor the ring of zero. The
alphabet is therefore eight glyphs. The saltire contributes zero to both S and W while
reading as a non-digit, so a padded group's guards read as if the pad places were empty.
Unpadding is length-based from the table of contents, with the saltire as the in-band
end-of-data signal.

The glyph shapes are chosen, not left to the pipeline. Measurement validates their visual
distance rather than choosing them: the digits and the saltire must separate at the
required minimum distance on the measured channel, with digit zero against digit six the
pair to watch. A failing pair returns to the operator as a mitigation or a recorded
fallback, never a silent change.

## Consequences

- A group holds up to six data columns and two guards over base seven.
- The alphabet is eight glyphs, one more than the radix, because the pad is distinct.
- The fallback, if eight glyphs cannot be separated on the measured channel, is to merge
  the pad into digit zero for seven glyphs and lose the in-band end-of-data signal, or to
  drop to base five with a distinct pad for six glyphs and shorter groups of four data
  columns. Both fallbacks keep a prime base, so the distance-three guarantee holds.
- The interleave depth and the resulting guard count stay gated on the channel measurement
  (call/0010).
