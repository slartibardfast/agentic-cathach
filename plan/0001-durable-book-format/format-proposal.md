# Format proposal: decisions and sequence

- Status: accepted; decisions recorded in call/0009 through call/0015
- Scope: cathach
- Date: 2026-07-05
- Serves: [Bríd](../../cast/brid-archivist.md) (primary), and the wider cast

**This proposal is the accepted design record that produced the milestone's decisions. Those
decisions are now the immutable record, in [call/0009](../../call/0009-payload-symbols-and-guards.md)
through [call/0015](../../call/0015-longevity-and-positioning.md), which govern. This document is
kept as the frozen working record they were extracted from; where the two differ, the decisions
win.**

## Purpose and status

This proposal consolidates the prior-art survey ([prior-art.md](prior-art.md)), the cast
review in [poc-findings.md](poc-findings.md), and the advisor critique into one reviewable
document. It locks the decisions the record supports, marks what stays gated on the unrun
physical measurement, and sequences the remaining work. It is a proposal, not the format
spec, and it does not rewrite either preamble draft.

The operator has ruled on the six forks this proposal raised. Those rulings are folded in:
the former open questions are now settled decisions, and the summary table and the affected
subsections carry the settled values. The one revision this leaves open for a later round is
the normative spec, not this document.

The primary persona's central objection governs the whole document: a reader must never
mistake a placeholder for a settled parameter. Every choice below is therefore labelled
**decided** or **gated**, and a decided choice states the basis it rests on. Where a decided
principle has a gated parameter inside it (interleaving is decided, its depth is not), the
split is stated explicitly.

One housekeeping defect remains and is assigned to the sequence below rather than silently
fixed here: [poc-findings.md](poc-findings.md) presents itself as the data-gathering result
for `#measure-channel`, but its numbers come from a software simulation. No physical
measurement has occurred. The document needs relabelling so its status is honest. The
earlier print-resolution contradiction is now closed by ruling. The single target is 600
dpi (decision below), which supersedes the README's 300 DPI lock.

## Summary

| Decision | Choice | Status | Basis |
|---|---|---|---|
| Radix | Base 7 (prime); alphabet is digits 0 to 6 plus a saltire pad | Decided; shapes hand-approved; distance validation gated | prior-art.md, S and W verdict; operator glyph ruling |
| Group size | Six data columns per group (base minus one) | Decided | Weight aliasing over the modulus |
| Redundancy code | Reed-Solomon at roughly one-third overhead | Decided | Peer survey; burst failure model |
| Decode mode | Erasure mode, driven by an image-derived damage map | Decided | Reed-Solomon distance economics |
| Layout | CIRC-style interleaving; a group's glyphs never contiguous | Principle decided; depth gated | CIRC; dvdisaster; Optar |
| Guard scheme | Keep both S and W | Decided | Check-digit record |
| Guard count | Erasures to recover per group, plus one | Rule decided; count gated | prior-art.md, guard-count logic |
| Padding and the zero mark | Distinct pad glyph, a saltire; alphabet is eight glyphs; unpadding is length-based and taught | Decided; distance validation gated | ToC-free end-of-data; Cormac's guard-value point |
| Integrity digests | Hand tier a two-digit running pair; machine tier SHA-256 as printed digits; both per file and per book | Decided | Tier budgets; Fintan's rebuild check |
| Preamble pedagogy | Multiplication taught as arrays before the weighted guard, plus the survey's idioms | Decided | Teaching record; call/0008 breach |
| Positioning | Centuries on ISO 9706 paper; novelty is the hand tier, the preamble, erasure decode | Decided | Archival record |
| Finder set | Two-tier distributed registration: error-correcting fiducial markers plus an interior timing mesh | Decided; amends call/0004 | prior-art.md, finder-robustness section |
| Print resolution | 600 dpi single target | Decided; supersedes README 300 DPI | call/0004; module-vs-capture |
| Alphabet distance validation, burst width, interleave depth, RS/repetition split | None | Gated | Await `#measure-channel`, `#design-alphabet` |

## The decisions

### The radix is base 7

**Decided (operator ruling):** the payload base is seven, a prime, and the eight-glyph
alphabet it needs is now hand-approved. **Gated:** only the distance validation of those
chosen shapes on the measured channel, since the radix and the glyph-alphabet size are one
decision. The padding ruling below makes the alphabet eight glyphs, and the operator has
exercised the hand-approval gate and chosen them.

**The glyph shapes are hand-approved.** Values zero to six are the Hindu-Arabic digits
0 1 2 3 4 5 6 (candidate A from the preview), and the distinct pad glyph is a saltire, a
diagonal cross drawn as the two diagonals of the cell, chosen for reading plainly as neither
a digit nor the ring of zero. The hand-approval gate that Bríd's audit stance and the
operator's authority over glyph decisions demanded is now satisfied, not pending, so no
agent or algorithm chose the alphabet. A benefit of the digit family is worth stating: the
payload now shares one alphabet with the preamble, which already teaches those digits, so
the reader learns one set of marks.

Measurement's remaining role is to validate these chosen shapes, not to choose them.
`#design-alphabet` confirms that the digits zero to six and the saltire hold their minimum
visual distance on the measured channel and flags any confusable pair. The pair to watch is
digit zero against digit six, which share a rounded stroke that damage can close. If a pair
proves too close under damage, that returns to the operator as a mitigation or the recorded
eight-glyph fallback, never a silent change.

The hand-tier check pair, S as the plain sum and W as the position-weighted sum, both mod
the base, is the RAID-6 dual-parity Reed-Solomon construction. That construction is a
genuine distance-three code only over a field, which for these moduli means a prime base
(prior-art.md, "The verdict on the S and W scheme and the base";
https://igoro.com/archive/how-raid-6-dual-parity-calculation-works/).

Base six is composite, and the survey enumerates what it silently forfeits: detection of
jump transpositions across columns two, three, or four apart, recovery of two located
erasures, and correction of one unlocated error. These are exactly the powers the weighted
guard exists to add. Base six also aliases column weights, so a group caps at five data
columns. What survives base six is only what the plain sum already secures. Base seven is
prime, costs one more glyph, restores every forfeited power, and allows six data columns.
Base five is prime but caps a group at four data columns, likely too short.

A worked base-seven group, for review of the shape (values illustrative):

```host-lint:ignore
positions        1   2   3   4   5   6        (six data columns, base 7)
data             3   6   1   5   4   2

S = (3+6+1+5+4+2) mod 7                      = 0
W = (1*3+2*6+3*1+4*5+5*4+6*2) mod 7          = 0

distance three over the prime base:
  - any two located erasures recoverable (machine or rebuilt tool)
  - one unlocated error correctable (machine or rebuilt tool)
  - every transposition detected, adjacent or jump
```

**Contingency.** The eight-glyph budget the padding ruling creates is the one remaining
risk to base seven, and its fallbacks are recorded with the padding decision below. Both
fallbacks keep a prime base (seven with the pad merged into digit zero, or five with a
distinct pad), so the distance-three guarantee holds in either. A composite base is not on
the path: advertising the full distance-three guarantee over base six would be false, since
its weighted guard is not a code over a field, and this proposal rules that route out
(prior-art.md, hand check digits; https://en.wikipedia.org/wiki/Check_digit).

### Redundancy is Reed-Solomon, decoded in erasure mode

**Decided:** the machine tier uses Reed-Solomon at roughly one-third overhead, and the
decoder runs in erasure mode, fed by an image-derived per-module damage map. **Gated:** the
exact split between Reed-Solomon and any repetition layer, which the channel measurement
fixes.

Reed-Solomon over LDPC is settled by the failure model: cathach's damage is bursty
(cracks, flaking, liquid rings), and even JAB Code's own authors report Reed-Solomon
beating their LDPC on burst errors (prior-art.md, paper backup survey). One-third overhead
sits at the safe end of the mainstream range: QR's strongest level is near thirty percent,
dvdisaster runs fifteen to thirty-three, Optar's Golay is a heavy fifty.

Erasure mode is the one place cathach beats the existing paper-backup tools. An RS(n,k)
code has minimum distance n - k + 1 and corrects e errors with s erasures when
2e + s < d: a located erasure costs one unit of distance where a blind error costs two, so
the same parity budget recovers twice as many located erasures
(https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction). Contiguous toner
damage announces its location, and a per-module confidence map (PaperBack's quality map is
the precedent) turns that announcement into erasure flags. The surveyed tools mostly treat
all damage as unlocated error and leave the two-to-one saving unclaimed (prior-art.md,
lessons taken). Claiming it is the format's genuine technical edge.

**Layout, decided in principle:** follow CIRC. Never lay a group's glyphs contiguously.
Interleave so the largest expected burst, measured in glyph widths, divided by the
interleave depth, stays within the group's recoverable-erasure count. Use a light inner
check to localize damage and flag erasures, and a de-interleaved outer group to recover
from the flags (prior-art.md, CIRC;
https://en.wikipedia.org/wiki/Cross-interleaved_Reed%E2%80%93Solomon_coding). The depth
itself is gated: it derives from the measured worst-case burst width. Group size is capped
at base minus one data columns by weight aliasing, as above.

### Keep both guards, and count them by the erasure target

**Decided:** the group keeps both S and W. An unweighted sum catches no transposition,
because a sum does not depend on order, so dropping W would blind the hand tier to the most
common transcription error (prior-art.md, hand check digits). The hand-tier guarantee is
scoped honestly to what subtraction alone delivers: recover one located erasure, detect one
substitution, detect adjacent transpositions. Anything needing a modular inverse (two
erasures at once, an unlocated error) belongs to the scanner and rebuilt-tool tiers, where
the prime base makes it available.

**Rule decided, count gated:** to recover r located erasures per group and still detect one
further substitution takes r + 1 guard digits. Two guards is correct exactly when the
measurement shows damage erases at most one glyph per interleaved group. If rings or cracks
routinely erase two or more glyphs from one group even after interleaving, the count rises.
Both the worst-case burst width and the achievable interleave depth come from
`#measure-channel`, so the count cannot be fixed before it (prior-art.md, guard-count
logic).

### A distinct pad glyph, and length-based unpadding

**Decided (operator ruling).** The cast found the two drafts conflating three roles that
this decision now separates. Nuala's "absence mark"
([preamble-proposal-nuala.md](preamble-proposal-nuala.md)) served both as the empty-place
holder inside a positional number and as the end-of-data pad, while
[preamble-draft.md](preamble-draft.md) padded with digit zero. Neither draft teaches
unpadding, so a hand decode of a file whose digit stream leaves the last group partly filled
can emit spurious trailing values. The three roles, disambiguated:

- **Digit zero is a value.** It is a positive mark, never a blank (Nuala's rule stands: a
  blank cannot be told from damage).
- **The empty-place holder inside a positional number is digit zero.** The middle place of
  "2 0 5" is the value zero, and the same glyph carries it.
- **The end-of-data pad that fills out a short final group is a new, distinct glyph.** It is
  not digit zero and reads differently. The hand-approved shape is a saltire, the two
  diagonals of the cell, chosen to read plainly as neither a digit nor the ring of zero.

Only the third role takes the distinct mark. The alphabet is therefore **radix plus one
glyphs, which at base seven is eight**: the digits zero through six, plus the saltire pad
mark.

**The pad's value under the guards.** The saltire contributes zero to both S and W. It is
arithmetically zero, so a padded group's guards read as if the pad places were empty, and it
is visually a non-digit, so a reader tells end-of-data from a genuine zero value. The spec
must state this contribution explicitly (Cormac's point: the guard tier must define every
glyph's arithmetic).

**Unpadding stays length-authoritative.** The distinct pad is the visible in-band signal,
and it buys the hand decoder end-of-data detection without consulting the table of contents.
The authoritative rule stays the recorded length: the decoder keeps only as many values as
the file's recorded length and discards the rest of the final group. The manifest's per-file
length is already a locked constraint (README, locked constraints: byte fidelity is exact,
with explicit per-file length and a defined padding convention), so length-based unpadding
adds no new machinery. It adds a teaching obligation: **the preamble must teach both the
distinct pad as the end-of-data mark and the file length from the table of contents as the
authority.** No current draft does either, and the preamble regeneration step below carries
the fix.

**Compound contingency, gated on `#design-alphabet`.** Base seven and a distinct pad
together push the alphabet to eight glyphs, which raises the stakes on the alphabet
measurement. If `#design-alphabet` finds the paper and toner cannot separate eight glyphs at
the required visual distance, the fallbacks in order are: (i) merge the pad back into digit
zero, for seven glyphs, which loses the ToC-free end-of-data signal while length-based
unpadding still governs; or (ii) drop to base five with a distinct pad, for six glyphs and
shorter groups of four data columns. Both fallbacks keep a prime base, so the
distance-three guarantee holds either way.

### The digests, one per tier, both printed

**Decided (operator ruling): both digests print per file and per book.** The final printed
constants belong to the spec. Fintan's whole tier is verifying a rebuilt decoder against
what the page shows, so both digests must be specified exactly and printed concretely.

**Hand tier: a two-digit running pair in the payload base.** Over the data digits of the
covered span, in the reading order the corner marks fix, with guards excluded, keep two
accumulators A and B, both starting at zero. For each data digit d, first add d to A and
keep the ones place, then add A to B and keep the ones place. Print A then B. This needs
addition and the taught set-aside-full-groups step only, no multiplication and no division,
so it sits inside the hand budget. The second accumulator makes the pair order-sensitive,
so a transposition moves B even when A is unchanged. Its strength is honestly modest, a
transcription and tamper tripwire rather than cryptography, and the spec must say so. The
digest is printed in the table of contents, per file and for the whole book.

**Machine tier: SHA-256, printed as concrete digits.** Per file and per book, over the
exact bytes, printed in the book (the current samples show uppercase hex in the colophon;
the spec fixes the final printed form). No human computes it; a rebuilt tool confirms it in
a moment. The pairing follows the design's locked integrity constraint (README, locked
constraints) and the layered-integrity pattern in [architecture.md](architecture.md).

Excluding the guards from the hand digest is deliberate: a repaired row then re-checks
against the same printed digest, so repair and verification stay independent evidence.

### Preamble pedagogy

**Decided.** The survey's teaching record converges on a small set of idioms, and the
preamble regeneration step adopts all of them (prior-art.md, lessons taken for the
preamble):

- **Teach multiplication as arrays, before the weighted guard (operator ruling).**
  Multiplication is built as arrays and repeated equal groups, before any figure uses it.
  The weight is then presented as "copy each column's digit as many times as the strokes
  beneath it, and add". Every weight is at most the group length and every digit less than
  the base, so the arrays are tiny and fully drawable. This closes the floor breach:
  `call/0008` fixes the floor at counting, yet its budget names addition and
  multiplication, and no draft teaches multiplication. The reconciliation is recorded as a
  decision (below): the floor stays at counting, and addition and multiplication are
  taught, never assumed. An addition-only computation of the weighted guard exists (the same
  double running sum the hand digest uses), and it stays available as an implementation
  option, but the array figure is the taught method, because it teaches what the weight
  means.
- **An explicit units-position marker**, so reading direction and place value are fixed by
  the figure rather than inferred (the Arecibo precedent, and the nuclear-marker warning
  that direction must never be inferred).
- **Each value shown in more than one form at once**, dots, strokes, and glyph sharing one
  figure, so notation is welded to quantity and the three languages share the figure.
- **Each new symbol taught by many worked instances**, never one, and never by a figure
  that needs an implied verb.
- **A built-in decode self-test before the guard machinery**, the Voyager-cover pattern: a
  reader who parsed the numerals correctly reproduces a known figure, and a wrong parse
  visibly fails.
- **Keep the "a group-size is a choice" figure** from Nuala's draft. No base is universal,
  and the figure teaches the base as one arbitrary choice.
- **Teach length-based unpadding**, per the padding decision above.

### Positioning and honest scope

**Decided.** The longevity claim is centuries on ISO 9706 permanent paper with monochrome
toner, dark-stored, recoverable with no special hardware and trivially copyable. It is not
a metal-etch millennium, and the substrate, not the toner, is the limit (prior-art.md,
archival lessons). The genuine novelty is the unaided pen-and-paper hand tier, the
self-describing preamble on commodity print, the bound-book form, and erasure-mode decoding
against a named physical failure model. The coding core (Reed-Solomon, interleaving,
registration) is prior art to reuse, never reinvent.

### Two-tier distributed registration

**Decided (operator ruling).** The native dense field carries two registration tiers:
error-correcting fiducial markers (STag, ChArUco, or AprilTag style, about two to three
millimetres) at the page corners and edges for global orientation and homography, and an
interior mesh of dedicated timing marks refined to sub-pixel precision for local grid pitch
and page curl.

The reason is a single-point-of-failure argument. Corner-only finders kill the whole read
when damage lands on the finder, which is the registration fragility that makes PaperBack
fail in practice, and QR's own finder patterns carry no error correction. A distributed mesh
resynchronizes locally, so damage to one mark stays regional, and the mesh absorbs paper
stretch and curl (Optar's precedent). Error-correcting fiducial markers survive damage to a
marker, since a small lattice still fixes orientation and warp when any one marker is lost
(prior-art.md, "Finder and registration robustness"; https://arxiv.org/abs/1707.06292 ;
https://www.mdpi.com/2076-3417/10/21/7814 ; http://ronja.twibright.com/optar/). This amends
`call/0004`'s bare corner-registration stance, flagged below.

### Print resolution

**Decided (operator ruling): a single 600 dpi target.** It supersedes the README's 300 DPI
lock and closes the contradiction with `call/0004` and [architecture.md](architecture.md).

One consequence is recorded rather than left open. A 0.22 mm module at 600 dpi is about five
scanner pixels, which a flatbed reads natively but a phone camera reads only marginally on a
curved page (prior-art.md, module against capture). The dense tier therefore leans
flatbed-primary. The phone path Aoife wants asks for larger modules or several fused
captures, and that trade belongs to the alphabet and layout design, not to a new fork.

## What stays measurement-gated

The radix value is decided at seven and the glyph shapes are hand-approved, but the
alphabet's distance still awaits validation on the measured channel, and none of the
following is settled here. No document should present a working value for them as final:

- **The distance validation of the hand-approved alphabet.** The digits zero to six and the
  saltire pad are chosen, but `#design-alphabet` must confirm they separate at the required
  visual distance on the measured channel and flag any confusable pair, digit zero against
  digit six chief among them. A pair that proves too close returns to the operator as a
  mitigation or the recorded fallback (seven glyphs by merging the pad, or base five with a
  distinct pad), never a silent change.
- **The worst-case burst width** on real paper, toner, and printer, which fixes the
  interleave depth and, through the r + 1 rule, the guard count.
- **The split between Reed-Solomon and repetition**, confirmed rather than assumed by the
  channel measurement.

All hang on `#measure-channel` and `#design-alphabet`. The base-six values in the POC, the
drafts, and the architecture stub are working placeholders and must read as such.

## The sequence

The critical path, each step with its gate. Steps earlier in the list block the steps that
cite them; the spec step runs in parallel with the first two.

1. **Author the measurement protocol and its decision table.** The protocol document
   specifies the prints, the damage regimen, the scan, and the tallies; the decision table
   maps each plausible measured band (substitution rate, burst width, erasure rate) to a
   radix, group size, guard count, and interleave depth before any ink is spent, so the
   measurement reads out a decision rather than opening a debate. Needs no hardware.
   Gate: operator approves the protocol and the table covers every plausible band.
2. **Run `#measure-channel`.** Print, damage, scan, tally, on the real paper, toner, and
   printer. The only step needing hardware. While relabelling for this step, fix
   [poc-findings.md](poc-findings.md) so it presents as the simulation it is.
   Gate: measured substitution rate, erasure rate, and worst-case burst width recorded.
3. **Nail the normative format spec.** The settled parts are writable now and run in
   parallel with the two steps above: the distinct-pad and guard-value convention, the
   length-based unpadding rule, the digest definitions, the header and table-of-contents
   fields, and the 600 dpi lock that supersedes the README's 300 DPI. The alphabet-dependent
   constants land after the measurement. Gate: spec review passes with no contradiction
   open.
4. **Run `#design-alphabet`.** The shapes are hand-approved (digits zero to six plus the
   saltire pad), so this step validates rather than chooses. Print and scan the eight glyphs,
   confirm they separate at the required visual distance on the measured channel, and flag
   any confusable pair, digit zero against digit six first.
   Gate: the eight glyphs hold their minimum visual distance at the measured rate. A pair
   that fails goes to the operator for a mitigation or the recorded fallback (seven glyphs by
   merging the pad, or base five with a distinct pad), never a silent change.
5. **Regenerate the preamble's concrete content** from the settled spec. The pedagogical
   skeleton is already banked in the two drafts; what regenerates is the worked figures,
   the code table, the digest section, and the unpadding teaching.
   Gate: cast review, with Nuala's figure-carries-the-load standard applied.
6. **Blind hand-decode trial.** Someone who has never seen the spec recovers a page using
   only the printed preamble. This is the preamble's own test, and it appears in no current
   plan; it should be added to the milestone's build sequence as a task.
   Gate: the decoder succeeds without help, or every failure becomes a preamble fix.
7. **Run `#scope-rust-pipeline`.** Author the `.allium` behaviour spec before the code and
   wire the allium lane per the project's specification-driven discipline.
   Gate: the spec lane runs in CI and the obligations are dispositioned.

**Today sits before the first step.** The protocol document does not exist, no physical
measurement has run, and the existing artifacts are the survey, a software simulation, two
preamble drafts, and the stub architecture.

## Decisions to record in call/ once accepted

Named here for allocation by the lifecycle tool after operator acceptance; none is written
in this proposal:

- **Base 7 and the guard construction.** The base-seven radix, the group-size cap of six
  data columns, and the eight-glyph alphabet with its recorded fallbacks. Supersedes the
  base-six working assumption wherever it reads as settled.
- **The hand-approved alphabet.** Values zero to six as the Hindu-Arabic digits, the pad as a
  saltire, the operator hand-approval that chose them, and measurement's role as distance
  validation rather than choice. Records that the payload shares one alphabet with the
  preamble's teaching.
- **Redundancy and erasure-mode decoding.** Reed-Solomon at one-third, the damage-map
  erasure decode, and CIRC-style interleaving as layout law.
- **Padding and the pad glyph.** A distinct pad mark, a saltire, separate from digit zero,
  its zero contribution to S and W, and length-based unpadding as the authoritative rule and
  a taught obligation. Disambiguates the value, the empty-place holder, and the end-of-data
  pad.
- **Integrity digests.** The hand-tier running pair and the machine-tier SHA-256, both
  printed per file and per book, with the hand digest's honest scope.
- **Preamble pedagogy and the floor reconciliation.** The teaching order and idioms, and
  the amendment to `call/0008`: the floor stays counting, and addition and multiplication
  are taught, never assumed. Recorded as a new decision that amends 0008, since records are
  immutable.
- **Longevity and novelty positioning.** The centuries claim and the honest-novelty
  statement, so marketing and spec never drift apart.
- **Two-tier distributed registration.** Error-correcting fiducial markers plus an interior
  timing mesh. Amends `call/0004`'s bare corner-registration stance.
- **Print resolution.** The single 600 dpi target, recorded so the README lock and
  `call/0004` agree, with the flatbed-primary consequence.

## Resolved rulings

The six forks this proposal raised are ruled and folded into the decisions above, and the
operator has since exercised the glyph hand-approval gate. Recorded here for the reviewer's
trace:

1. **The base: seven.** Decided at base seven, with the eight-glyph alphabet now
   hand-approved and only its distance validation left to `#design-alphabet`.
2. **Multiplication: taught as arrays** before the weighted guard, `call/0008` amendment
   standing. The addition-only computation is kept as an implementation option, not the
   taught method.
3. **The pad glyph: distinct.** A separate end-of-data mark at radix plus one (eight glyphs
   at base seven), zero under both guards, with length-based unpadding authoritative and a
   compound feasibility contingency recorded.
4. **Finders: two-tier distributed registration.** Error-correcting fiducial markers plus an
   interior timing mesh, an amendment to `call/0004`.
5. **Print resolution: 600 dpi**, single target, superseding the README's 300 DPI, with the
   flatbed-primary consequence recorded.
6. **Digest coverage: per file and per book.**

The follow-on glyph hand-approval gate is now exercised too. Values zero to six are the
Hindu-Arabic digits, and the pad is a saltire, both chosen by the operator. Measurement
validates the shapes rather than choosing them, and the payload shares one alphabet with the
preamble's teaching.
