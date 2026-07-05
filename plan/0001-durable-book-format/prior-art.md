# Prior art survey for the durable book format

- Status: reference
- Scope: cathach
- Date: 2026-07-05

This survey grounds the milestone before the measurement protocol and the format spec
are written. It covers the five fields cathach touches: paper data backup and 2D
symbologies, long-term archival projects and media, self-describing messages that teach
a naive reader, erasure codes and hand-computable checks, and numeral systems with the
pedagogy of teaching number from scratch. It was gathered from primary sources; the
source links sit at the end of each part.

Two results carry weight for decisions the milestone has left open, so they are stated
up front and then supported below:

- The hand-tier check pair (S = sum, W = position-weighted sum, both mod the base) is the
  RAID-6 dual-parity Reed-Solomon construction. It is a genuine distance-three code only
  over a prime base. Base six is composite, so the weighted guard silently loses much of
  its strength. A prime base, seven for preference, restores it and allows a larger group.
  This bears directly on the open radix and on the guard design.
- The preamble uses multiplication in the weighted guard, but the assumed-knowledge floor
  is only counting, and no draft teaches multiplication. The record shows multiplication
  can be built from grouping and repeated addition, so the gap is closable without adding
  an axiom. This bears on the preamble draft and on [call/0008](../../call/0008-assumed-knowledge-floor.md).

## Paper data backup and 2D symbologies

The closest existing peers to cathach's machine tier. None offers a true hand-recoverable
tier, so the contrast is as useful as the overlap.

- **PaperBack (Oleh Yuschuk).** A full-page grid of black and white dots recovered by
  scanning, the nearest prior art to a native module field. About 500 KB per A4 at 600 dpi
  (theoretical; far less in practice), needs a 900 dpi or better scanner, Reed-Solomon with
  configurable block-erasure recovery and a per-region quality map. Widely reported as
  fragile in practice through alignment and registration failure, which is the failure mode
  a finder must beat. https://ollydbg.de/Paperbak/
- **Twibright Optar.** 200 KB per A4, extended Golay(24,12) at fifty percent overhead, data
  interleaved across twenty-four image strips so a local blot hits many words by one bit
  each. A checkerboard mesh of registration crosses with sub-pixel refinement. The canonical
  academic-grade peer, and its interleave-across-the-page design is the direct answer to
  contiguous damage. http://ronja.twibright.com/optar/
- **Data Matrix (ECC200).** Up to about 1,556 bytes, reconstructs at roughly thirty percent
  symbol damage if the finder survives. States the erasure economics explicitly: one check
  codeword corrects one located erasure against two for an unlocated error. An "L" finder on
  two sides plus a timing pattern on the other two. https://en.wikipedia.org/wiki/Data_Matrix
- **QR Code.** The battle-tested finder vocabulary: three corner finders with a distinct
  1:1:3:1:1 ratio (scale and rotation invariant), timing patterns for grid pitch, alignment
  patterns for warp. Reed-Solomon at four levels up to about thirty percent, blocks
  interleaved so a blot spreads across blocks. Its top level is below cathach's one-third
  target, over a small symbol rather than a page. https://www.qrcode.com/en/about/error_correction.html
- **dvdisaster.** An optical-disc scheme, and the reference for localized optical damage, the
  same physical class as cathach. Reed-Solomon with deep interleaving across the whole medium so
  a scratch becomes a few correctable erasures per codeword; the drive's own bad-sector
  reports act as erasure flags. https://en.wikipedia.org/wiki/Dvdisaster
- **PaperKey (David Shaw).** The human-transcribable exception. Prints an OpenPGP secret key
  as hex text with a CRC per row and a whole-key checksum, recovered by a person retyping
  until the checks match. The design pattern for a hand tier: short checksummed rows a person
  can transcribe. https://www.jabberwocky.com/software/paperkey/
- **Aztec and MaxiCode.** Central bullseye finders that are rotation invariant, need no quiet
  zone, and read omnidirectionally, which suits a module photographed at an angle on a curved
  page. https://en.wikipedia.org/wiki/Aztec_Code
- **JAB Code (Fraunhofer).** Colour matrix at about three times mono density, but its own
  authors note Reed-Solomon beats their LDPC on burst errors. Since cathach's damage is
  bursty, this argues for staying Reed-Solomon. https://jabcode.org/

Lessons taken:

- Stay Reed-Solomon, not LDPC, for a bursty failure model. One-third overhead is mainstream
  and toward the safe end, not extravagant. QR tops out near thirty percent, dvdisaster
  runs fifteen to thirty-three percent, Optar's Golay is a heavy fifty.
- Interleave deeper than the worst expected defect, so the largest plausible blot touches
  only a correctable number of symbols per codeword. Every scheme that survives bursts
  shares this move.
- Detect erasures and decode in erasure mode. This is cathach's biggest available edge and
  the field's blind spot: the theory is standard, but paper-backup tools mostly treat all
  damage as unlocated error and leave the two-to-one saving on the table. A per-module
  confidence map, as in PaperBack's quality map, feeds it.
- Reuse a proven finder set: the QR triad of finder, timing, and alignment marks, or a
  bullseye. Coarse finders alone are what makes PaperBack fail and QR succeed.
- Honest novelty. A scanned module field, Reed-Solomon at a third, interleaving, and a
  printed text fallback beside the code are all prior art. The differentiated parts are the
  true hand tier, the self-describing preamble that carries the codec, the bound-book form,
  and the active use of located erasures against a named physical failure model.

## Long-term archival projects and media

- **Long Now Rosetta Disk.** A micro-etched nickel disk whose front is a spiral of text that
  starts eye-readable at the rim and shrinks to microscopic, so the taper itself says
  "magnify me, there is more." The outer ring names the archive, gives the date, states the
  scope, and tells the reader the tool needed. This is cathach's exact preamble pattern, done
  well. https://rosettaproject.org/disk/concept/
- **Piql and the Arctic World Archive.** Photosensitive film holding data frames plus
  human-readable text, where "all information required to recover the data is written on the
  film itself in human-readable text, along with the source code for the retrieval software".
  Recovery needs only a light source, a lens, a camera, and a computer. https://www.piql.com/
- **GitHub Arctic Code Vault.** The same film, with a per-reel index, a decode guide in five
  languages, and a separate Tech Tree reel that explains computing down to pre-industrial
  foundations so a reader can rebuild the missing context. A model for choosing a small
  language set deliberately and for a context layer beyond the terse decode header.
  https://archiveprogram.github.com/arctic-vault/
- **Paper and fused laser toner.** cathach's actual medium. The honest ceiling is decades to
  a few centuries, gated by the paper rather than the toner, which is inert. Governing
  standards are ISO 9706 and ISO 11108 for permanent paper and ISO 11798 for the permanence
  of printing. The claim to make is "recoverable for centuries with no special hardware and
  trivially copyable", not a metal-etch timescale. https://www.loc.gov/preservation/resources/rt/perm/pp_5.html
- **Memory of Mankind, Project Silica, M-DISC.** Ceramic reading by naked eye but carrying no
  dense payload; glass and disc reading only by bespoke machine. They bracket the trade cathach
  sits inside: unaided readability against density.

Lessons taken:

- The graduated-scale, self-teaching opener is the proven idiom. Name the artifact, date it,
  state the scope, and tell the reader how to read the rest, in plain language, before any
  dense encoding.
- Carry the codec on the medium, never in an external key. Every project that expects to be
  read makes the artifact self-sufficient, the way PDF/A forbids external dependencies.
- Realistic longevity for paper and toner is centuries, and the substrate is the limit.
  Specify ISO 9706 paper and monochrome toner and claim honestly.
- A fully unaided pen-and-paper hand tier appears genuinely distinctive. Every dense-data
  project surveyed assumes at least a lens or a scanner.

## Self-describing messages and teaching a naive reader

The field closest to the preamble's real problem: teaching a stranger to read from a low
floor, language-independently.

- **Place-value pedagogy and non-verbal teaching.** The acquisition order is additive, then
  base grouping, then multiplicative, then positional. Physical bundling into a base, shown
  paired with the written numeral, is what makes place value conceptual. A controlled study
  found first-person demonstration matched or beat verbal instruction for arithmetic, and that
  a single word could break a task that non-verbal instruction completed, but that a "which is
  bigger" comparison failed without words. So a figure that encodes an act is safe; a figure
  that needs an implied verb is not. https://pmc.ncbi.nlm.nih.gov/articles/PMC6028808/
- **Lincos (Freudenthal).** Teaches notation by ostension: enough true worked examples to kill
  ambiguity, numbers first in unary then in positional form shown beside the counted quantity,
  multiplication taught like addition by volume of examples. https://en.wikipedia.org/wiki/Lincos_language
- **Arecibo message.** Places a units-position marker beneath the digits so reading direction
  and place value are fixed rather than guessed, and teaches the base by displaying its full
  run. The caution: none of the scientists Drake first sent it to fully decoded it, so no
  single clever cue can be load-bearing. https://en.wikipedia.org/wiki/Arecibo_message
- **Cosmic Call (Dutil and Dumas).** The closest analog to cathach's shared figures: each
  number shown at once as a group of dots, a positional glyph, and its value, with the four
  operations taught by worked equations and an error-resistant glyph font. https://www.plover.com/misc/Dumas-Dutil/messages.pdf
- **Pioneer plaque and Voyager cover.** The Voyager cover includes a built-in self-test: the
  first image is a circle, so a reader who decoded the raster correctly gets a circle and a
  wrong aspect ratio gives an ellipse. The critique (Gombrich) is that the arrow and
  perspective are learned conventions a stranger will not share. https://science.nasa.gov/mission/voyager/golden-record-cover/
- **Nuclear-waste warning markers.** The field that studied cross-millennial communication
  most rigorously and mostly doubted it is solvable. Its lessons: a sequence read the other way
  can reverse a warning into a promise, so reading direction must never be inferred; every
  convention drifts, so meaning must be layered and redundant, never carried by one pictograph;
  and solemn monuments can attract rather than repel. https://en.wikipedia.org/wiki/Long-term_nuclear_waste_warning_messages

Lessons taken for the preamble:

- Build the number ladder in the proven order: unary tally with a terminator, then bundling
  into a base shown as figures, then positional digits with an explicit units marker, then the
  base displayed as its full run, then multiplication as arrays, then the weighted guard. The
  guard's multiplication must not precede a figure that builds multiplication.
- Present each value in more than one form at once, the way Cosmic Call pairs dots, tally, and
  glyph. This welds notation to quantity and lets the three languages share one figure.
- Teach each new symbol by many worked instances, not one, and never by a figure that needs an
  implied verb.
- Add a built-in decode self-test the reader can reproduce only if they parsed the numerals
  right, placed before the guard machinery.
- Never require the reader to infer direction or orientation. Mark the units place, frame
  numbers so they self-delimit, and avoid arrows and perspective.

## Erasure codes and hand-computable checks

The technical core, and the source of the base result.

- **Reed-Solomon and the erasure economics.** An RS(n,k) code has minimum distance
  d = n - k + 1 and corrects e errors with s erasures when 2e + s < d. A located erasure costs
  one unit of distance, an unlocated error costs two, so a fixed parity budget recovers twice
  as many located erasures as blind errors. This is the whole basis of cathach's "damage
  announces its location" bet, and it validates the one-third overhead. https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction
- **CIRC, the CD code.** Two Reed-Solomon layers with a convolutional interleaver between them:
  a weak inner code flags uncertain bytes as erasures, the de-interleaver scatters each block's
  bytes across many outer blocks, and the outer code recovers using those flags. A contiguous
  gouge of several millimetres is corrected because interleaving turns one burst into a few
  spread-out erasures. The template for cathach's page layout. https://en.wikipedia.org/wiki/Cross-interleaved_Reed%E2%80%93Solomon_coding
- **RAID-6 dual parity.** Two parity symbols, P as a plain sum and Q as a position-weighted
  sum over a field. The result is a distance-three code that recovers any two located erasures
  or corrects one unlocated error. This is exactly the shape of cathach's S and W. https://igoro.com/archive/how-raid-6-dual-parity-calculation-works/
- **Hand check digits.** A plain sum mod the base catches any single substitution but no
  transposition, because a sum does not depend on order. A position-weighted sum catches
  transpositions, but only where the weight differences are coprime to the modulus. ISBN-10 uses
  a prime modulus of eleven and so catches every single error and every transposition; the
  UPC and Luhn schemes over composite ten miss the transposition of digits differing by five.
  Verhoeff and Damm reach full coverage over composite ten only by leaving linear arithmetic for
  a group or quasigroup, at the cost of hand-computability. https://en.wikipedia.org/wiki/Check_digit

### The verdict on the S and W scheme and the base

Write the group as data digits over a base b, with S = sum of the digits mod b and
W = sum of (column index times digit) mod b. This pair is the RAID-6 dual-parity
Reed-Solomon construction, a shortened generalized Reed-Solomon code that is MDS with
minimum distance three over a field, that is, when the base is prime.

Base six is composite, and the integers mod six are not a field. That breaks the code in
enumerable ways. What survives base six, because the plain sum carries it, is: recovering one
located erasure by subtraction alone with no division, detecting any single substitution, and
detecting any adjacent transposition. What base six silently forfeits, because it needs
invertibility the composite modulus lacks, is: detecting jump transpositions across columns
two, three, or four apart, recovering two located erasures, and correcting one unlocated
error. These are exactly the powers the weighted guard was meant to add beyond the plain sum.
The weight is also taken mod six, so column six gets weight zero and column seven aliases
column one, which caps a base-six group at five data columns.

So base six is adequate only for the modest claim the plain sum already secures, and the
second guard is close to wasted over it. A prime base removes every blind spot and makes S and
W a genuine distance-three code: every transposition is caught, any two located erasures are
recoverable, and one unlocated error is correctable. Base seven is prime, one glyph larger than
six, and supports groups up to six data columns. Base five is prime but caps groups at four,
likely too short. The recommendation, if the glyph-alphabet measurement can bear one more
symbol, is to prefer base seven. If the alphabet is pinned to six by the paper and toner
measurements, keep six but scope the advertised guarantee to what the plain sum secures, or
replace the linear weighted guard with a Verhoeff or Damm style digit and accept the loss of
hand-computability.

The guard count follows the same logic. To recover r located erasures per group and still
detect one further substitution needs r + 1 guard digits. Two guards is therefore correct for a
target of one erasure per group plus one detection, and is right only if the paper measurements
show damage erases at most one glyph per interleaved group. If rings or cracks routinely erase
two or more glyphs from one group even after interleaving, the count rises to r + 1. This is
genuinely measurement-gated, since both the worst-case burst width and the achievable
interleave depth come from the channel measurement.

The layout follows CIRC. Never place a group's glyphs contiguously. Interleave so the largest
expected burst, a ring diameter or a crack width in glyph widths, divided by the interleave
depth, stays within the group's recoverable-erasure count. Use a light inner check to localize
damage and flag erasures, and a de-interleaved outer group to recover from the flags. A
row-and-column product layout lets a blob be corrected by iterating the two axes.

## Numeral systems, zero, and teaching number from scratch

- **The floor is well founded.** One-to-one tallying is attested to about 44,000 years ago and
  recurs independently across cultures, and subitizing to about four is innate, so "marks carry
  meaning and the reader can count" sits near the true universal. https://en.wikipedia.org/wiki/Tally_marks
- **Teaching, not assuming, positional notation is right.** A true positional zero was invented
  independently only about three times, in Babylon, among the Maya, and in India, and the Maya
  zero never spread. Rome was non-positional, and Hindu-Arabic place value reached Europe late
  and met resistance there, common only by the fifteenth century. Positional notation is a
  specific invention a far-future reader may not share, which vindicates [call/0008](../../call/0008-assumed-knowledge-floor.md).
  https://wals.info/chapter/131
- **No base is universal.** A world sample has decimal at about sixty-four percent, with
  vigesimal, senary, and body-part systems all attested and living. A reader may count in scores
  or by body parts, so the base must be taught as one arbitrary choice, which Nuala's "a group
  size is a choice" figure does. A composite base such as six is not culturally alien; the
  Ndom language counts in sixes. https://en.wikipedia.org/wiki/Senary
- **Reading order and number words differ.** German builds units before tens, Arabic reads
  numerals right to left, and East Asian number words are transparently decimal. Any prose that
  leans on spoken order or on "the left" teaches unevenly across the three languages, so the
  figures must carry the load.

The multiplication gap and its fix. The drafts invoke multiplication in the weighted guard but
never build it from grouping, while the floor is only counting. The fix is proven and
figure-native: teach multiplication as repeated equal groups, an array, before the guard, then
present the weight as "copy each column's digit as many times as the strokes beneath it, and
add". Every weight is at most five and every digit at most the base, so the arrays are tiny and
fully drawable, and nothing beyond grouping and adding is introduced. The weighted guard should
not be dropped, since an unweighted sum cannot catch transpositions. https://makemathmoments.com/progression-of-multiplication/

## Finder and registration robustness

A follow-up gather, prompted by the finder decision, extended the paper-backup survey above
with fiducial-marker systems and finder-damage behaviour it had not reached.

- **Corner finders are a single point of failure.** QR's three corner patterns, Data
  Matrix's L, and Aztec's central bullseye each fail the whole read when damage lands on the
  finder, and QR's finder patterns carry no error correction. This is the registration
  fragility that makes PaperBack fail in practice, since its grid registration is implicit
  and it needs the page near parallel and oversampled. For a bound page where a ring or
  crack can strike a corner, corner-only registration is the wrong choice.
  https://www.mdpi.com/2076-3417/10/21/7814
- **A distributed mesh resynchronizes locally.** Twibright Optar covers the page with a mesh
  of registration crosses refined to sub-pixel precision, so damage to one cross stays
  regional rather than global, and the mesh absorbs paper stretch and page curl. This is the
  paper-specific answer to whole-page registration. http://ronja.twibright.com/optar/
- **Error-correcting fiducial markers survive damage to the marker.** Purpose-built markers
  carry the resilience QR finders lack: STag has a selectable Hamming distance, a circular
  border for stable localization, and occlusion tolerance near a fifth of the marker;
  ChArUco is a chessboard with embedded markers that interpolates across a missing marker;
  AprilTag corrects several bit errors. A small lattice of these survives the loss of any one
  marker, because the rest still fix orientation and the warp. https://arxiv.org/abs/1707.06292
- **Module size against capture.** A module near 0.22 mm at 600 dpi is about five scanner
  pixels, which a flatbed reads natively but a phone camera reads only marginally on a curved
  page. The fiducial markers themselves are larger, so they are found regardless. The dense
  tier therefore leans on the flatbed, and the phone path wants larger modules or several
  fused captures.

The recommendation is two-tier registration: error-correcting fiducial markers at the page
corners and edges for global orientation and homography, and an interior mesh of dedicated
timing marks with sub-pixel refinement for local grid pitch and page curl. This amends
`call/0004`'s bare corner-registration stance.

## What this changes for cathach

Decisions and obligations this survey creates or sharpens, to feed the spec, the measurement
protocol, and the preamble:

- **Reconsider the base.** Base six is not merely unfixed, it is a weak choice for the weighted
  guard because it is composite. Prefer a prime base, seven for preference, unless the glyph
  measurement forces six. Record the base decision on the axes of durability, check strength, and
  teachability, and drop cultural neutrality as a criterion since no base is neutral. This raises
  the priority of measuring the glyph count in `#design-alphabet`, because the base and the
  alphabet size are the same decision.
- **Close the multiplication gap in the preamble** by teaching multiplication as arrays and
  repeated addition before the weighted guard, and by purging the word from any prose that
  precedes the figure. Reconcile this with [call/0008](../../call/0008-assumed-knowledge-floor.md),
  whose budget names addition and multiplication but whose floor names only counting.
- **Set the interleave and guard count from the measurement.** The channel measurement must
  yield the worst-case burst width so the interleave depth and the guard count can be derived,
  not guessed. Add these as explicit outputs of the measurement protocol.
- **Adopt the proven preamble idioms:** a graduated self-teaching opener that names, dates, and
  scopes the artifact, an explicit units marker, each value shown in more than one form, teaching
  by many worked instances, and a built-in decode self-test before the guard machinery.
- **Claim longevity honestly:** centuries on ISO 9706 paper with monochrome toner, dark-stored,
  recoverable with no special hardware, not a metal-etch timescale.
- **State the honest novelty:** the unaided hand tier, the self-describing preamble on commodity
  print, the book form, and the active use of located erasures. The coding core is well-trodden
  and should be reused, not reinvented. Erasure-mode decoding driven by an image damage map is the
  one place cathach can beat the existing paper-backup tools.

## Sources

Paper backup and symbologies: https://ollydbg.de/Paperbak/ ,
http://ronja.twibright.com/optar/ , https://www.jabberwocky.com/software/paperkey/ ,
https://github.com/za3k/qr-backup , https://en.wikipedia.org/wiki/Dvdisaster ,
https://www.qrcode.com/en/about/error_correction.html ,
https://en.wikipedia.org/wiki/PDF417 , https://en.wikipedia.org/wiki/Data_Matrix ,
https://en.wikipedia.org/wiki/Aztec_Code , https://jabcode.org/

Archival projects and media: https://rosettaproject.org/disk/concept/ ,
https://www.piql.com/ , https://arcticworldarchive.org/ ,
https://archiveprogram.github.com/arctic-vault/ , https://www.memory-of-mankind.com/ ,
https://www.loc.gov/preservation/resources/rt/perm/pp_5.html ,
https://www.microsoft.com/en-us/research/blog/project-silicas-advances-in-glass-storage-technology/

Self-describing messages: https://en.wikipedia.org/wiki/Lincos_language ,
https://en.wikipedia.org/wiki/Arecibo_message ,
https://www.plover.com/misc/Dumas-Dutil/messages.pdf ,
https://science.nasa.gov/mission/voyager/golden-record-cover/ ,
https://en.wikipedia.org/wiki/Long-term_nuclear_waste_warning_messages ,
https://pmc.ncbi.nlm.nih.gov/articles/PMC6028808/

Erasure codes and hand checks: https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction ,
https://en.wikipedia.org/wiki/Cross-interleaved_Reed%E2%80%93Solomon_coding ,
https://igoro.com/archive/how-raid-6-dual-parity-calculation-works/ ,
https://en.wikipedia.org/wiki/Check_digit ,
https://www.ece.unb.ca/tervo/ece4253/isbn.shtml ,
https://en.wikipedia.org/wiki/Verhoeff_algorithm , https://dl.acm.org/doi/pdf/10.1145/243439.243457

Numeral systems and pedagogy: https://en.wikipedia.org/wiki/Tally_marks ,
https://en.wikipedia.org/wiki/Ishango_bone , https://wals.info/chapter/131 ,
https://en.wikipedia.org/wiki/Senary , https://mayan.org/civilization/zero/ ,
https://mathshistory.st-andrews.ac.uk/HistTopics/Chinese_numerals/ ,
https://makemathmoments.com/progression-of-multiplication/ ,
https://pmc.ncbi.nlm.nih.gov/articles/PMC4462644/
