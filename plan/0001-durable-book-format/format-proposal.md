# Format proposal: decisions and sequence

- Status: proposed, for operator review
- Scope: cathach
- Date: 2026-07-05
- Serves: [Bríd](../../cast/brid-archivist.md) (primary), and the wider cast

## Purpose and status

This proposal consolidates the prior-art survey ([prior-art.md](prior-art.md)), the cast
review in [poc-findings.md](poc-findings.md), and the advisor critique into one reviewable
document. It locks the decisions the record already supports, marks what stays gated on the
unrun physical measurement, and sequences the remaining work. It is a proposal, not the
format spec, and it does not rewrite either preamble draft.

The primary persona's central objection governs the whole document: a reader must never
mistake a placeholder for a settled parameter. Every choice below is therefore labelled
**decided** or **gated**, and a decided choice states the basis it rests on. Where a decided
principle has a gated parameter inside it (interleaving is decided, its depth is not), the
split is stated explicitly.

Two housekeeping defects surfaced during consolidation and are assigned to the sequence
below rather than silently fixed here:

- [poc-findings.md](poc-findings.md) presents itself as the data-gathering result for
  `#measure-channel`, but its numbers come from a software simulation. No physical
  measurement has occurred. The document needs relabelling so its status is honest.
- The print resolution is stated twice and differently: the milestone README locks a
  300 DPI print target, while [architecture.md](architecture.md) and `call/0004` put the
  dense machine tier at 600 dpi with a 0.22 mm module. The normative spec must resolve
  which number is the lock, or state that the two registers print at two resolutions.

## Summary

| Decision | Choice | Status | Basis |
|---|---|---|---|
| Radix family | Prime base; seven preferred | Decided (family); exact value gated | prior-art.md, S and W verdict |
| Group size | Data columns per group at most base minus one | Decided | Weight aliasing over the modulus |
| Redundancy code | Reed-Solomon at roughly one-third overhead | Decided | Peer survey; burst failure model |
| Decode mode | Erasure mode, driven by an image-derived damage map | Decided | Reed-Solomon distance economics |
| Layout | CIRC-style interleaving; a group's glyphs never contiguous | Principle decided; depth gated | CIRC; dvdisaster; Optar |
| Guard scheme | Keep both S and W | Decided | Check-digit record |
| Guard count | Erasures to recover per group, plus one | Rule decided; count gated | prior-art.md, guard-count logic |
| Padding and the zero mark | One glyph; the pad is digit zero; unpadding is length-based and taught | Decided, flagged as a fork | Round-trip constraint; glyph budget |
| Integrity digests | Hand tier a two-digit running pair; machine tier SHA-256 as printed digits | Shape decided | Tier budgets; Fintan's rebuild check |
| Preamble pedagogy | Multiplication taught before the weighted guard, plus the survey's idioms | Decided | Teaching record; call/0008 breach |
| Positioning | Centuries on ISO 9706 paper; novelty is the hand tier, the preamble, erasure decode | Decided | Archival record |
| Finder set | Reuse a proven finder vocabulary | Decided; amends call/0004 | PaperBack failure against QR success |
| Exact radix, glyph shapes, burst width, interleave depth, RS/repetition split | None | Gated | Await `#measure-channel`, `#design-alphabet` |

## The decisions

### The radix is prime, seven preferred

**Decided:** the payload base is a prime, with seven the recommendation. **Gated:** the
final value, because the radix and the glyph-alphabet size are one decision, settled
together in `#design-alphabet` against the measured substitution rate.

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

**Contingency, if measurement forces six.** If `#design-alphabet` finds the paper and
toner cannot bear a seventh glyph at the required visual distance, keep base six but do one
of two things honestly: scope the advertised guarantee down to what the plain sum secures
(one located erasure recovered, one substitution detected, adjacent transpositions
detected), or replace the linear weighted guard with a Verhoeff or Damm style digit, which
reaches full detection over a composite base at the cost of hand-computability
(prior-art.md, hand check digits; https://en.wikipedia.org/wiki/Check_digit). Advertising
the full distance-three guarantee over base six would be false, and this proposal rules it
out.

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

### One zero glyph, and length-based unpadding

**Decided now, radix-independent.** The cast found a contradiction between the drafts:
[preamble-draft.md](preamble-draft.md) pads the last group with the digit zero, while
[preamble-proposal-nuala.md](preamble-proposal-nuala.md) pads with an "absence mark" that
its own teaching introduces as the empty-place holder, which is the positional zero under
another name. The unpadding rule is missing from both drafts, so a hand decode of a file
whose digit stream leaves the last group partly filled can emit spurious trailing values.

The decision: **the absence mark and the digit zero are one glyph.** The alphabet holds
exactly radix glyphs. Zero is a positive mark, never a blank (Nuala's rule stands: a blank
cannot be told from damage). Length removes the padding: the decoder keeps only as many
values as the file's recorded length and discards the rest of the final group.
The manifest's per-file length is already a locked constraint of the design (README, locked
constraints: byte fidelity is exact, with explicit per-file length and a defined padding
convention), so length-based unpadding adds no new machinery. What it adds is a teaching
obligation: **the preamble must teach the reader to read the file's length from the table
of contents and keep only that many values.** No current draft does, and the preamble
regeneration step below carries the fix.

The basis for one glyph rather than two: the glyph alphabet is the format's scarcest
resource, since every added glyph shrinks the minimum visual distance the measurement must
protect. One more glyph is better spent buying the prime base seven than a pad marker whose
job the manifest already performs. A distinct pad glyph would also need a defined numeric
value under S and W in any case, so it saves the reader nothing.

This is a real fork and is listed in the open questions: the alternative (a distinct
absence glyph, radix plus one glyphs, value zero under the guards) lets a hand decoder
recognise end-of-data without consulting the table of contents, at the cost of one glyph of
alphabet budget.

### The digests, one per tier, both printed

**Shape decided now; final constants belong to the spec.** Fintan's whole tier is verifying
a rebuilt decoder against what the page shows, so both digests must be specified exactly
and printed concretely.

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

- **Teach multiplication before the weighted guard.** Multiplication is built as arrays and
  repeated equal groups, before any figure uses it. The weight is then presented as "copy
  each column's digit as many times as the strokes beneath it, and add". Every weight is at
  most the group length and every digit less than the base, so the arrays are tiny and
  fully drawable. This closes the floor breach: `call/0008` fixes the floor at counting,
  yet its budget names addition and multiplication, and no draft teaches multiplication.
  The reconciliation is recorded as a decision (below): the floor stays at counting, and
  addition and multiplication are taught, never assumed.
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
finder patterns) is prior art to reuse, never reinvent.

One consequence amends an existing decision. `call/0004` gives the native dense field "a
thin border and three corner registration marks" and "no finder patterns". The survey's
verdict is that coarse finders alone are what makes PaperBack fail where QR succeeds, and
the lesson is to reuse a proven finder vocabulary: the QR triad of finder, timing, and
alignment marks, or a rotation-invariant bullseye (prior-art.md, lessons taken). This
proposal adopts finder reuse and flags the `call/0004` amendment below.

## What stays measurement-gated

None of the following is decided here, and no document should present a working value for
them as settled:

- **The exact radix** among the prime candidates, and with it the final group size. The
  alphabet and the radix are one decision, made in `#design-alphabet`.
- **The glyph shapes and the alphabet size**, drawn to maximise minimum visual distance
  against the measured silent-substitution rate.
- **The worst-case burst width** on real paper, toner, and printer, which fixes the
  interleave depth and, through the r + 1 rule, the guard count.
- **The split between Reed-Solomon and repetition**, confirmed rather than assumed by the
  channel measurement.

All four hang on `#measure-channel` and `#design-alphabet`. The base-six values in the POC,
the drafts, and the architecture stub are working placeholders and must read as such.

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
3. **Nail the normative format spec.** The radix-independent parts are decidable now and
   run in parallel with the two steps above: the padding and zero decision, the digest
   definitions, the header and table-of-contents fields, and the DPI contradiction between
   the README lock and `call/0004`. The radix-dependent constants land after the
   measurement. Gate: spec review passes with no contradiction open.
4. **Run `#design-alphabet`.** Apply the prime-base decision and its contingency to fix
   the radix, and draw the glyphs against the measured substitution rate.
   Gate: the chosen alphabet meets the required minimum visual distance at the measured
   rate.
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

- **Prime radix and the guard construction.** The radix family, the seven-preferred
  recommendation, the forced-six contingency, and the group-size cap. Supersedes the
  base-six working assumption wherever it reads as settled.
- **Redundancy and erasure-mode decoding.** Reed-Solomon at one-third, the damage-map
  erasure decode, and CIRC-style interleaving as layout law.
- **Padding and the zero mark.** One glyph, positive mark, length-based unpadding as a
  taught obligation.
- **Integrity digests.** The hand-tier running pair and the machine-tier SHA-256, both
  printed, with the hand digest's honest scope.
- **Preamble pedagogy and the floor reconciliation.** The teaching order and idioms, and
  the amendment to `call/0008`: the floor stays counting, and addition and multiplication
  are taught, never assumed. Recorded as a new decision that amends 0008, since records are
  immutable.
- **Longevity and novelty positioning.** The centuries claim and the honest-novelty
  statement, so marketing and spec never drift apart.
- **Finder reuse.** Amends `call/0004`'s no-finder-pattern stance.
- **Print resolution.** Whichever way the DPI contradiction resolves, recorded so the
  README lock and `call/0004` agree.

## Open questions for the operator

The real forks. Each has a recommendation above; the operator's ruling closes it.

1. **The base.** Rule on seven (recommended) against five, and on the forced-six
   contingency: if the alphabet measurement pins the radix at six, choose between scoping
   the guarantee down to the plain sum's powers and replacing the weighted guard with a
   Verhoeff or Damm digit that costs hand-computability.
2. **Multiplication in the hand tier.** Rule on teaching multiplication as arrays before
   the weighted guard (recommended), against redesigning the guard to avoid multiplication
   entirely. One observation feeds this: the weighted sum admits an addition-only
   computation, the same double running sum the digest uses, whose accumulated weights are
   an equivalent set over a prime base. If that becomes the taught method, the weighted
   guard needs no multiplication at all and the floor amendment could stop at addition.
   The recommendation stands with arrays, because the array figure teaches what the weight
   means, but the fork is genuine and affects both the preamble length and `call/0008`.
3. **The absence mark.** Rule on one glyph with length-based unpadding (recommended),
   against a distinct pad glyph at radix plus one, which buys ToC-free end-of-data
   detection for a hand decoder at the cost of one glyph of alphabet budget.
4. **Finder patterns.** Confirm the amendment of `call/0004`: adopt a proven finder set
   for the native dense field in place of the bare corner registration marks.
5. **Print resolution.** Rule on the DPI lock: 300 DPI as the README states, 600 dpi as
   `call/0004` assumes, or an explicit two-register rule with each register at its own
   resolution.
6. **Digest coverage.** Confirm the hand digest prints per file and for the whole book
   (recommended), or narrow it to one of the two.
