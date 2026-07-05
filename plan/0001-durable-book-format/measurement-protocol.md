# Measurement protocol for the print-and-damage channel

- Status: design parameters set; awaiting the hardware parameters and print approval
- Scope: cathach
- Date: 2026-07-05
- Serves: [Bríd](../../cast/brid-archivist.md) (primary), and the wider cast

This document is the protocol for the `#measure-channel` task in the milestone build
sequence ([README.md](README.md)). It fixes the error-correction parameters and validates
the hand-approved glyph alphabet by measurement on real paper, toner, and printer, before
the Rust pipeline is scoped. It is the root of the critical path.

The earlier [poc-findings.md](poc-findings.md) is a software simulation. It proves the
base-N arithmetic round-trips and works a hand recovery, but it never touched a printer. This
protocol is the physical measurement, and its numbers come from ink on paper, not from code.

## 1. Purpose and what it fixes

The measurement resolves the parameters the format cannot fix by assumption. Each is tied to
a recorded decision:

- **The silent-substitution rate.** How often damage turns one printed glyph into a
  different valid glyph, per glyph and per damage mode. This drives the guard design and the
  Reed-Solomon budget (call/0010).
- **The located-erasure rate.** How often damage destroys a glyph recognisably, so its place
  is known but its value is gone. The erasure economics of call/0010 rest on this being the
  common case, since a located erasure costs one unit of code distance where a silent error
  costs two.
- **The worst-case burst width**, measured in glyph widths, from the real failure modes. This
  sets the interleave depth, and through the erasures-plus-one rule it sets the guard count
  (call/0009, call/0010).
- **The visual-distance validation of the eight-glyph alphabet.** The alphabet is
  hand-approved: the digits 0 to 6 for the values and a saltire for the distinct pad
  (call/0009). Measurement validates that these eight glyphs hold their minimum visual
  distance on the channel and flags any confusable pair. Digit zero against digit six is the
  pair to watch, since they share a rounded stroke that damage can close.
- **The eight-glyph feasibility, confirmed or fallen back.** If the eight glyphs cannot be
  separated on the measured channel, the recorded fallbacks apply: merge the pad into digit
  zero for seven glyphs, or drop to base five with a distinct pad for six glyphs (call/0009).

The measurement chooses none of these by itself. It reads out a decision already written in
the decision table below, and a glyph failure returns to the operator rather than changing
silently, through the gate below.

## 2. Specimen design

The specimen is a set of printed test pages carrying the real format elements at candidate
sizes. The exact counts are operator parameters, listed at the end; the content is fixed here.

- **The eight glyphs at candidate module sizes.** Print the digits 0 to 6 and the saltire
  pad at the four module sizes fixed below (0.17 to 0.34 mm at 600 dpi, bracketing
  0.22 mm, call/0014), so the measurement spans the size the dense tier will use and one step
  smaller and larger. Each glyph appears
  many times at each size, in isolation and adjacent to every other glyph, so a confusable
  pair shows up in context.
- **Guarded groups in the base-7 row shape.** Lay the glyphs in guarded groups of up to six
  data columns plus the two guards S and W (call/0009), so the specimen exercises a real row
  and a real located-erasure recovery, not just isolated marks.
- **The two registers.** Print the naked-eye instructional register at its reading size and
  the dense payload register at its candidate module sizes (call/0014), so the substitution
  and erasure rates are measured in the register that will carry data.
- **The two-tier registration marks.** Include the error-correcting fiducial markers at the
  page corners and edges and an interior mesh of timing marks (call/0011), so the damage
  battery can test whether registration survives a hit that would kill a corner-only finder.

## 3. The damage battery

The insults model cathach's stated failure model of spatially contiguous damage
(prior-art.md, erasure economics and CIRC interleaving; call/0010). For each mode, the
specimen is damaged in a controlled way and then measured for two things: whether the damage
announces its location as an erasure, or whether it produces a silent substitution into
another valid glyph.

- **Cracking and creasing along folds.** Fold and unfold the sheet along ruled lines through
  glyph rows and through registration marks. Measure the width of the damaged band in glyph
  widths and whether toner loss along the crease reads as an erasure.
- **Flaking and abrasion.** Rub and scrape the toner over a marked area. Measure whether a
  partly removed glyph is recognisably gone (erasure) or altered into another glyph
  (substitution).
- **Liquid rings.** Apply a liquid ring over a group and let it dry. Measure the ring
  diameter in glyph widths and the state of the glyphs under the ring.
- **Toner spread and bleed.** Where the print or the paper causes toner to spread, measure
  whether adjacent glyphs merge and whether a spread stroke closes the zero-against-six gap.
- **Light fading.** Expose a specimen to light for a controlled dose. Measure whether a faded
  glyph stays recognisable, degrades to an erasure, or shifts toward another glyph.
- **Edge and corner loss.** Tear or abrade an edge and a corner, and target one corner that
  carries a fiducial marker. Measure whether the remaining fiducial markers and the interior
  mesh still register the page (call/0011).

## 4. Capture and tally

Each damaged specimen is captured and then tallied glyph by glyph.

- **Capture.** Scan on a flatbed at 600 dpi, the primary path (call/0014). Also photograph
  with a phone camera on a flat page and on a curved page, to record how far the dense tier
  reads off a flatbed and how marginally it reads off a phone. The phone result is judged
  against the convenience bar fixed below.
- **Tally, per glyph and per damage mode.** Record how often a damaged glyph is read as a
  different valid glyph (a silent substitution), how often it is destroyed recognisably (a
  located erasure), and how often it survives. Record the confusion pairs, with the count for
  zero against six kept separately.
- **Burst measurement.** For each contiguous insult, record the size of the largest damaged
  region that still decodes, in glyph widths, both before and after interleaving is modelled.
  This is the worst-case burst width that feeds the decision table.

## 5. The decision table

The table maps each plausible measured band to a settled parameter before any ink is spent,
so the measurement reads out a decision rather than opening a debate. The bands and the
thresholds within them are operator parameters, listed at the end; the mapping is fixed here.

The keys are the measured silent-substitution rate, the located-erasure rate, and the
worst-case burst width in glyph widths. The outputs are the radix confirmation or fallback,
the group size, the guard count, the interleave depth, and the Reed-Solomon versus
repetition split.

```host-lint:ignore
KEYS (measured)                          OUTPUTS (settled)
--------------------------------------   --------------------------------------------------

alphabet visual distance
  8 glyphs separate at required dist  -> radix stays base 7, alphabet 8 glyphs (0..6 + pad)
  only 7 separate (pad ~ a digit)     -> merge pad into zero: 7 glyphs, lose in-band EOD
  only 6 separate                     -> base 5 with distinct pad: 6 glyphs, groups of 4
  a value pair fails (e.g. 0 vs 6)    -> to operator: mitigation or recorded fallback

silent-substitution rate (per glyph)     bands set below: low <0.1%, moderate 0.1-1%, high >1%
  low                                 -> two guards suffice for detection; RS budget nominal
  moderate                            -> hold two guards; raise RS share within one-third
  high                                -> to operator: alphabet or size change before coding

located-erasure rate (share of damage located)     boundary set below: high >=90%, low <90%
  high (damage announces location)    -> decode in erasure mode as designed (call/0010)
  low (much damage silent)            -> to operator: the erasure-mode edge is weakened

worst-case burst width  B  (glyph widths, post-interleave target)
  erasures per group  r = ceil(B / D)   for interleave depth D
  guard count                          = r + 1                     (call/0009)
  group data columns                   <= base - 1 (6 at base 7, 4 at base 5)
  interleave depth  D                  chosen so r stays within group's guard budget
  RS vs repetition
    bursts corrected by RS+interleave  -> Reed-Solomon at ~one-third overhead
    residual whole-glyph loss remains  -> add repetition only for that residue
```

The interleave depth D is set so that the worst-case burst B, spread across D, leaves at most
r erasures in any one group, where the guard count r + 1 stays inside the group's budget of
base minus one columns. A larger measured B forces a deeper interleave, a smaller group, or a
higher guard count, in that order of preference, since a deeper interleave costs no code
distance.

## 6. Gates and receipt

- **Approval before printing.** The operator approves this protocol and the decision table
  before any specimen is printed, so the run reads out decisions rather than inviting new
  ones.
- **Task receipt.** The run records the `#measure-channel` task receipt through the lifecycle
  tool, per the milestone build sequence ([README.md](README.md)).
- **Glyph failure returns to the operator.** A failed glyph pair, zero against six above all,
  goes to the operator for a mitigation or a recorded fallback under call/0009. The alphabet
  is hand-approved, so measurement validates it and never changes it silently.

## 7. Design parameters, set

The operator ruled the design-side parameters on 2026-07-05. The hardware parameters stay
open below.

- **Module sizes.** Four candidate modules at 600 dpi on clean dot counts: 0.17 mm (4 dots),
  0.21 mm (5 dots, the target near 0.22 mm), 0.25 mm (6 dots), and 0.34 mm (8 dots, a
  legibility reference). The instructional register sits at 3.0 mm cap-height and is tested
  down to the 2.8 mm naked-eye floor.
- **Accept thresholds.** The band cut points that key the decision table above are
  set. The silent-substitution bands sit at 0.1 and 1 percent per glyph, and the
  located-erasure boundary at 90 percent. The visual-distance floor is 99.9 percent correct
  discrimination for every glyph pair under no damage, and the zero-against-six pair is the
  acceptance gate.
- **Sample counts.** A full run up front, for a tenth-of-a-percent resolution on the
  per-glyph substitution rate. The run prints about 500 reads per glyph per module size per
  damage mode at a fixed moderate dose, which aggregates to about 12,000 reads per glyph. It
  adds a three-dose severity sweep per damage mode at the 0.21 mm target module, and about
  3,000 undamaged reads per glyph per module size for the discrimination baseline. About 30
  damaged specimens carry the registration battery, keyed on whether the two-tier finders
  still register the page.
- **Phone-capture bar.** A convenience bar. A flat-page phone photo in good light must
  register the two-tier finders and decode the coarsest 0.34 mm module. A full dense decode
  off a phone is not required, since the flatbed is the primary path (call/0014).

## 8. Open parameters, for the bench

Two physical choices stay with the operator and are made with the hardware in hand.

- **Paper stock.** The exact ISO 9706 permanent paper stock or stocks to test, within the
  longevity positioning of call/0015.
- **Printer and toner.** The printer model and the monochrome toner, since the failure modes
  and the substitution rate are specific to them.
