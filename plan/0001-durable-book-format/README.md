# Durable book format for cathach

- Status: proposed
- Scope: cathach
- Persona: [Bríd](../../cast/brid-archivist.md), the archivist-preservationist,
  is the primary persona this milestone serves; the full cast is in `cast/`.

## Goal

Settle, by measurement, the parameters the printed-book encoding cannot fix by
assumption, then scope the cross-platform Rust pipeline that implements the settled
format. The near-term work is data gathering: the error-correction split, the
erasure depth per group and per page, and the glyph alphabet all come from running
the protocol on real paper, toner, and printers. This milestone imports the format
design below and sequences that measurement work ahead of any code.

The design is imported from a research note (`cathach-design.md`, 2026-07). It is
reproduced here as the milestone's design context; the open items it lists become
the build sequence at the end.

## Design: a durable printed-book encoding

Cathach encodes an arbitrary file tree as a printed book that decodes back to exact
bytes. The book is normally machine-scanned, but any region stays recoverable by a
human with pen and paper once the printed preamble is read. The print target is
PDF/A-3 on a common black-and-white laser printer at 300 DPI, read by unaided eye
with no lens.

*Cathach, "the Battler", is the oldest surviving Irish manuscript, a sixth-century
Vulgate psalter traditionally attributed to Colum Cille and carried by the Ó
Domhnaill as a protective relic. The name fits a book format built to outlast the
machines that read it. (The etymology is worth a second check before it reaches
print.)*

### Decoding tiers

The format decodes at three tiers of falling capability and rising durability.
Correctness lives in the specification. The tools are a re-derivable expression of
it, and the decoder's own C and Forth source ships inside the payload tree and again
as a PDF/A-3 attachment, so a reader can rebuild the tool tier from the human tier.

| Tier | Reader | Role |
|---|---|---|
| Scanner | Flatbed or camera scanner plus software | Fast bulk decode with full Reed-Solomon correction |
| Rebuilt tool | C or Forth tool rebuilt from the printed source | Reconstruction when the scanner tier is gone |
| Hand | Human, pen and paper, using the preamble | Last-resort recovery, expected in practice to recover only the rebuilt-tool bootstrap |

Whole-tree-by-hand stays a guarantee that is never expected to be exercised.

### Locked constraints

| Area | Decision |
|---|---|
| Decode direction | Decode-only for humans. Machine tooling is bidirectional: it encodes and scans back. The scan-back path may be lost, so the spec is authoritative. |
| Human maths budget | Addition and multiplication, taught in one-third of a page per language |
| Preamble languages | Three, selected by literate-speaker population |
| Correction | Preferred, with roughly one-third of each page given to redundancy |
| Longevity | The practical limit of high-quality paper and fused laser toner |
| Page loss | A lost page is recoverable from the rest. Every page is assumed to survive partially with limited local damage. |
| Redundancy scope | Within-book |
| Byte fidelity | Exact, with explicit per-file length and a defined padding convention |
| Document integrity | SHA-256 for the machine tier, plus a hand-checkable digest for the human tier |
| Visual registers | Two: a plain instructional register, and a dense payload register |
| Print target | PDF/A-3, black-and-white laser, 300 DPI, no lens |
| Container | A file tree with an embedded manifest holding paths, per-file lengths, and per-file hashes |

### Redundancy model

Within-book. Physical damage to fused toner is spatially contiguous: cracking,
flaking, and liquid rings take out regions rather than scattered independent dots.
Such damage presents as an erasure with a known location rather than a silent error,
and an erasure is far cheaper to correct. The machine tier uses Reed-Solomon erasure
coding at roughly one-third overhead, which the measurement below is expected to
confirm beats plain repetition for this failure shape.

The human tier is capped at one erasure per group, recoverable by subtraction alone
and so inside the addition-and-multiplication budget. It is worked below, with each
symbol a single digit in the payload base (base 10 shown for legibility) and both
checks kept in that base so no division is ever needed:

```
positions          1   2   3   4   5
data               3   7   1   9   4

plain check   S  = (3+7+1+9+4) mod 10           = 4     # fills a located erasure
weighted chk  W  = (1·3+2·7+3·1+4·9+5·4) mod 10 = 6     # detects a silent substitution

-- damage erases position 3, value unknown, location known --
positions          1   2   3   4   5
data               3   7   ?   9   4

known sum        = (3+7+9+4) mod 10   = 3
recovered v3     = (S - known) mod 10 = (4 - 3) mod 10 = 1     # exact, subtraction only
```

Two erasures at once, or a blind error with no known location, require dividing by a
position difference. That needs a modular inverse, which is outside the human budget,
so those cases belong to the scanner and rebuilt-tool tiers. The same subtraction
logic recovers one fully lost page from a cross-page parity page, one lost page per
parity group.

### Integrity, layered

Per group, two linear checks in the payload base. The plain sum fills a located
erasure using subtraction. The position-weighted sum detects a silent substitution
using multiplication and addition. Neither uses division, so both stay teachable
within the preamble budget.

Per document, SHA-256 for the machine tier, printed as glyphs and embedded as an
attachment. A hand-checkable running-sum digest covers the human tier, since no
human computes SHA-256 on paper.

### Symbol alphabet

Small radix, four to eight. Two forces push it low: the cost of teaching the alphabet
trilingually, and the visual distance needed between glyphs so that damage lands a
glyph in the cheap erasure regime instead of turning it into a different valid glyph.
Density pushes it high, and legibility caps it. Naked-eye discrimination floors a
human glyph near 2.8 mm against roughly 0.34 mm for a 300 DPI machine module, about
an eight-to-one gap. The two visual registers follow directly: a plain instructional
register carries the preamble, page headers, and orientation marks, and a dense
payload register carries the data. The machine and human layers are therefore printed
as separate representations at their own scales.

### Open items

The assumed-knowledge floor is unresolved. Everything the preamble can define depends
on what a far-future reader is presumed to already know, and that presumption is not
yet fixed. Two candidate floors are in play: a minimal one, that marks carry meaning
and that a reader can count; and a higher one that also assumes positional numerals.
This is the deepest gap and it gates the preamble draft.

The error-correction parameters are fixed by measurement, not by assumption. The
final split between repetition and Reed-Solomon, and the target erasure depth per
group and per page, come from running the protocol on the actual paper, toner, and
printer.

The glyph alphabet is undesigned. The exact radix within four to eight, and the glyph
shapes that maximise minimum visual distance, are drawn against the measured
silent-substitution rate, since the two are coupled.

The third preamble language is open. English and Mandarin are settled by
literate-speaker population; the third is Hindi or Spanish pending a current figure.
Font embedding for PDF/A-3 needs Latin, Han, and then Devanagari or extended Latin.

## Build sequence

### Measure the print-and-damage channel {#measure-channel}

- verify: attested operator
- inputs: plan/0001-durable-book-format/README.md

Run the measurement protocol on the target paper, toner, and printer to fix the
erasure statistics, the silent-substitution rate, and the error-correction
parameters (the repetition and Reed-Solomon split, the erasure depth per group and
per page). This is the data-gathering the rest of the milestone waits on. Progress
and the current go/no-go gates are in [poc-findings.md](poc-findings.md).

### Design the glyph alphabet against the measurements {#design-alphabet}

- depends: #measure-channel
- verify: attested operator

Draw the glyph shapes and fix the radix within four to eight to maximise minimum
visual distance against the measured silent-substitution rate.

### Resolve the assumed-knowledge floor and draft the preamble {#draft-preamble}

- depends: #measure-channel
- verify: attested operator

Settle what a far-future reader is presumed to know, then draft the trilingual
preamble within the addition-and-multiplication budget. Fix the third language
(Hindi or Spanish) by a current literate-speaker figure.

### Scope the cross-platform Rust pipeline {#scope-rust-pipeline}

- depends: #design-alphabet, #draft-preamble
- verify: attested operator

Scope the cross-platform Rust software: a static folder-to-PDF/A-3 encoder, and a
bidirectional FUSE wrapper for Linux and macOS. Author the `.allium` behaviour spec
(and any `.tla` timing spec) before the code, per the project's specification-driven
discipline, and record the build in `.host-software` as per-platform builds.
