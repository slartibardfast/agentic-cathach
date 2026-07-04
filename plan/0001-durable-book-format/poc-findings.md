# POC findings: prerequisite data and the style/substance gates

- Status: for review (operator + cast)
- Scope: cathach
- Date: 2026-07-04
- Serves: [Bríd](../../cast/brid-archivist.md) (primary), and the wider cast

This is the data-gathering result for the `#measure-channel` and `#design-alphabet`
tasks. It grounds cathach's parameters in real data and in a working proof-of-concept,
then stops at the go/no-go gates that need the operator and the cast to decide (and, for
the physical numbers, a printer). The POC code is exploratory, not the spec-driven
pipeline; it lives in the cathach worktree at `software/cathach/poc/` and emits plain
text, so a reviewer reads, prints, and hand-checks it with no rendering step.

## What the POC does

It encodes a small multi-script Unicode corpus, "A Little Memory of Earth", into base-N
digit groups with two hand-computable check digits per group, proves the bytes round-trip
exactly, and prints a sample page plus a worked erasure recovery. Unicode is the deliberate
sample: to read any preserved text a far-future reader first needs the character encoding,
so a compact Unicode slice is the bootstrap worth preserving.

The generated sample page (verbatim):

```host-lint:ignore
A LITTLE MEMORY OF EARTH

Corpus text : 🌍 中 अ Ω   (15 UTF-8 bytes)
  U+1F30D  EARTH GLOBE EUROPE-AFRICA
  U+4E2D   CJK UNIFIED IDEOGRAPH 中
  U+0905   DEVANAGARI LETTER A
  U+03A9   GREEK CAPITAL LETTER OMEGA

Encoding: each byte -> 4 base-6 digits; rows of 5 data digits + 2 checks S, W.
  S = (sum of the 5 data digits) mod 6            fills one located erasure
  W = (sum of position*digit, positions 1..5) mod 6   detects a substitution

  grp | d1 d2 d3 d4 d5 | S  W
  ----+----------------+------
    0 | 1  0  4  0  0  | 5  1
    1 | 4  2  3  0  3  | 0  2
    2 | 5  2  0  3  5  | 3  4
   ... (12 groups total; round-trip decode == original bytes: PASS)
```

The worked hand recovery (verbatim):

```host-lint:ignore
damaged row :  1  0  ?  0  0 | S=5 W=1     (a smudge erased position 3)
known sum of surviving data = 1
recovered   = (S - known) mod 6 = (5 - 1) mod 6 = 4     (exact, subtraction only)
```

Reproduce with `cargo run` in `software/cathach/poc/`; it writes `out/sample-page.txt`,
`out/erasure-demo.txt`, and `out/alphabet.txt`.

## Substance, grounded in data

- **Erasure coding, not error coding.** A Reed-Solomon `[n,k]` code corrects `n-k`
  erasures at known locations but only `(n-k)/2` errors at unknown ones. Contiguous
  toner damage announces its location, so cathach treats damage as located erasures and
  buys twice the correction per check symbol. The POC realises the hand-tier floor of
  this: one check digit fills one located erasure by subtraction.
- **The human arithmetic works and is exact.** The base-6 linear checks recover a located
  erasure with addition and subtraction only, and the round-trip reproduces the exact
  bytes across Latin, Greek, Han, and Devanagari. No division is ever needed.
- **Overhead is tunable.** The sample layout carries 2 checks per 5 data digits (40%).
  Two checks per 6 data digits give 33%. That meets the design's one-third target, in the
  range QR's strongest level uses (about 30%).
- **Density is deliberately conservative.** Comparable paper formats reach 200 kB/A4
  (Optar, Golay) to a claimed 500 kB/A4 (PaperBack, RS) at 600 dpi. cathach targets
  300 dpi and unaided-eye recovery, so it will hold far less per page. That is the price
  of durability and hand-decodability, paid on purpose.
- **The sizes have headroom.** A 2.8 mm human glyph sits well above the ~1.6 mm floor
  where 0.4 mm strokes merge, and above the 0.2 to 0.3 degree acuity threshold at reading
  distance, with margin for aged eyes and damage. A 0.34 mm machine module is about four
  dots at 300 dpi, which a camera reads without trouble.

The one number the POC cannot supply is the real one: the actual erasure and
silent-substitution rates on true paper, toner, and printer. That needs a physical
print-damage-scan test, which is a gate below.

## Style, pinned to two decisions

**Payload alphabet.** Two candidates:

- **A. Hindu-Arabic digits** (used in the samples). Every literate person already knows
  them, which shrinks the assumed-knowledge floor: the preamble teaches the arithmetic,
  never the alphabet. They are a reliable, beautiful workhorse set and read unambiguously
  in a good typeface. They are not damage-distance-optimal (0/6/8, 1/7, 3/8 confuse when a
  stroke is lost), but holding the radix at N<=6 keeps 7, 8, and 9 out of play.
- **B. A bespoke high-distance glyph set** (radix 4 to 8). Purpose-drawn marks that
  maximise the minimum visual distance, so a smudge lands a glyph in the cheap erasure
  regime rather than turning it into another valid glyph. Denser and damage-optimal, but it
  must be taught, which raises the assumed-knowledge floor.

The recommendation is A for the human tier (unambiguous, universal, representative of
humanity) and B held in reserve for a dense machine tier where density outweighs teaching.

**Preamble typefaces.** The plain instructional register needs beautiful, reliable,
unambiguous typefaces that represent humanity's scripts. The standing candidate is the
**Noto** family, whose remit is to render every Unicode script: Noto Serif for Latin, Noto
Serif CJK for Han, and Noto Serif Devanagari, which covers the trilingual preamble the
design calls for.

## Through the cast's eyes

- **Bríd** (archivist, primary): the round-trip proof and the hand-checkable parity give
  her an auditable fidelity claim she can defend to a trustee, and digits plus Noto make
  the object explainable to a lay board.
- **Oisín** (hand-decoder): digits, no division, and taught-arithmetic-not-alphabet give
  him the lowest assumed-knowledge floor. The worked recovery is exactly his procedure.
- **Fintan** (tool-rebuilder): the base-N scheme is small enough to reimplement from the
  printed description and check against the printed examples.
- **Cormac** (engineer): the POC's encode, decode, and round-trip assertion are the seed of
  the behaviour spec's obligations for the real Rust pipeline.
- **Aoife** (casual reader): the machine tier must decode from a phone camera with no app.
  Digits are camera-legible, and the coffee-table beauty rides on the typeface and glyph
  aesthetics. Her open question is whether the dense machine layer scans from a phone at
  300 dpi print.

## Go / no-go gates

Decide these before the spec-driven pipeline starts:

1. **Alphabet.** Digits (A) or bespoke glyphs (B) for the human payload tier? Recommend A.
2. **Radix and overhead.** N=6 with 2 checks per 5 or 6 data digits (40% or 33%)?
3. **Preamble typefaces.** Adopt the Noto Serif family for the trilingual preamble?
4. **Physical measurement.** Run the print-damage-scan protocol on the real paper, toner,
   and printer to fix the true erasure and silent-substitution rates and the final
   correction split. This needs hardware and is the gate the POC cannot pass alone.
5. **Scope.** On a go, promote the POC into the spec-driven Rust pipeline: author the
   `.allium` behaviour spec from the encode/decode obligations (and a `.tla` spec if the
   scan-back ordering needs one), then commit cathach and record its build.
