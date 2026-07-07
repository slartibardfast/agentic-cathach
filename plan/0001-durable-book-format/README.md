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

The physical parameters shown in the reproduced note are historical. Where they
differ from the Governing decisions section below, that section is authoritative:
the print target is 600 dpi ([call/0014](../../call/0014-print-resolution.md)), not
the 300 DPI the note locks, and the payload base is seven
([call/0009](../../call/0009-payload-symbols-and-guards.md)), not the small radix the
note leaves open.

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

### Governing decisions

The design above was imported from a research note, and its open questions have since been
settled and recorded as decisions, which govern this milestone:

- The assumed-knowledge floor is tally strokes ([call/0008](../../call/0008-assumed-knowledge-floor.md)),
  with the budget's arithmetic taught from that floor, not assumed
  ([call/0013](../../call/0013-preamble-pedagogy-and-floor.md)), and with subtraction,
  the repair's own operation, named in the budget and taught the same way
  ([call/0016](../../call/0016-subtraction-taught-from-the-floor.md)).
- The payload base is seven, with a hand-approved alphabet of the digits 0 to 6 and a saltire
  pad, eight glyphs in all ([call/0009](../../call/0009-payload-symbols-and-guards.md)). The
  glyph shapes are hand-approved, and the measurement validates their visual distance.
- Redundancy is Reed-Solomon decoded in erasure mode
  ([call/0010](../../call/0010-redundancy-and-erasure-decoding.md)); registration is two-tier
  ([call/0011](../../call/0011-two-tier-registration.md)); the digests are one per tier
  ([call/0012](../../call/0012-integrity-digests.md)); the print resolution is 600 dpi
  ([call/0014](../../call/0014-print-resolution.md)); the positioning is centuries on ISO 9706
  paper ([call/0015](../../call/0015-longevity-and-positioning.md)).
- The third preamble language is Spanish ([call/0007](../../call/0007-preamble-languages.md)).

What stays genuinely gated on the physical measurement: the split between repetition and
Reed-Solomon, the erasure depth per group and per page, the interleave depth and guard count
from the worst-case burst width, and the visual-distance validation of the eight-glyph
alphabet. The protocol that fixes them is [measurement-protocol.md](measurement-protocol.md).
The prior-art evidence behind the decisions is [prior-art.md](prior-art.md), and the accepted
design record they were extracted from is [format-proposal.md](format-proposal.md). A snapshot
of what remains undone before measurement is [undone-review.md](undone-review.md). The cast's
review of the milestone from the humanist standpoint is
[humanist-review.md](humanist-review.md).

## Build sequence

### Measure the print-and-damage channel {#measure-channel}

- verify: attested operator
- inputs: plan/0001-durable-book-format/README.md

Run the protocol in [measurement-protocol.md](measurement-protocol.md) on the target
paper, toner, and printer to fix the erasure statistics, the silent-substitution rate, and
the error-correction parameters (the repetition and Reed-Solomon split, the erasure depth
per group and per page), and to validate the eight-glyph alphabet. This is the data-gathering
the rest of the milestone waits on. The simulation and the go/no-go gates are in
[poc-findings.md](poc-findings.md); the working format design is in
[architecture.md](architecture.md).

### Design the glyph alphabet against the measurements {#design-alphabet}

- depends: #measure-channel
- verify: attested operator

The radix and the alphabet are decided ([call/0009](../../call/0009-payload-symbols-and-guards.md)):
base seven, digits 0 to 6 and a saltire pad, hand-approved. This task validates that
hand-approved alphabet against the measured silent-substitution rate, and takes the recorded
fallback if the eight glyphs cannot be separated at the required visual distance.

### Resolve the assumed-knowledge floor and draft the preamble {#draft-preamble}

- depends: #measure-channel
- verify: attested operator

The floor is settled at tally strokes ([call/0008](../../call/0008-assumed-knowledge-floor.md)).
Draft the trilingual preamble by the pedagogy in
[call/0013](../../call/0013-preamble-pedagogy-and-floor.md), which teaches the budget's
arithmetic from that floor, and teach subtraction from strokes before the first repair
figure uses it ([call/0016](../../call/0016-subtraction-taught-from-the-floor.md)). English
and Mandarin are joined by Spanish as the third language
([call/0007](../../call/0007-preamble-languages.md)). Two working drafts are under way, in
[preamble-draft.md](preamble-draft.md) and [preamble-proposal-nuala.md](preamble-proposal-nuala.md).
The gate includes a trilingual equivalence review: the three texts must carry identical
mathematical meaning, per [Nuala](../../cast/nuala-numeral-linguist.md)'s standard.

### Blind hand-decode trial {#blind-decode-trial}

- depends: #draft-preamble
- verify: attested operator

A person who has never seen the spec recovers a page from the printed preamble alone.
[format-proposal.md](format-proposal.md) names this trial as the preamble's own test, and
every failure becomes a preamble fix. One pass runs figure-only: the reader works from the
shared figures with the prose in a language they cannot read, which tests that the figures
carry the load and de-risks the language choice of
[call/0007](../../call/0007-preamble-languages.md) across the horizon of
[call/0015](../../call/0015-longevity-and-positioning.md).

### Scope the cross-platform Rust pipeline {#scope-rust-pipeline}

- depends: #design-alphabet, #draft-preamble
- verify: attested operator

Scope the cross-platform Rust software: a static folder-to-PDF/A-3 encoder, and a
bidirectional FUSE wrapper for Linux and macOS. Author the `.allium` behaviour spec
(and any `.tla` timing spec) before the code, per the project's specification-driven
discipline, and record the build in `.host-software` as per-platform builds.

The format spec includes printed known-answer tests: a worked specimen with its exact
bytes and both digests, so a rebuilt decoder is checked against the book's own pages
([Fintan](../../cast/fintan-tool-rebuilder.md)'s rebuild check). The browser-based decode
path that [Aoife](../../cast/aoife-casual-reader.md)'s modality rests on, a reader with
nothing installed, is out of this task's scope and is owed its own milestone once the
format settles.
