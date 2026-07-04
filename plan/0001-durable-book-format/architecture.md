# Architecture stub: the cathach book

This is the working architecture for the format, distilled from the POC and the review
sessions. The decisions it rests on are recorded in `call/`; the physical parameters wait
on the measurement gate in [poc-findings.md](poc-findings.md).

## Tiers of recovery

Capability falls and durability rises down the list. One coding family (base-N +
Reed-Solomon erasure) spans them, so the far-future rebuild teaches a single algorithm.

- **Read.** For text, the page is the readable text itself, with a per-line Fletcher-16
  checksum in the margin. No decoding.
- **Hand.** Base-N digit groups with the hand-computable checks; recovers a located erasure
  by subtraction. Confined to the preamble and a reading copy, never the bulk.
- **Tool.** The printed C and Forth source, rebuilt to decode the dense tier at machine speed.
- **Machine.** The dense cathach-native module field (`call/0004`): 64 KiB per A4 page at a
  0.22 mm module, 600 dpi, one-third to erasure coding. QR is a convenience layer only.

## Page types (`call/0005`)

- **Preamble.** Every tier, hand-recoverable. Teaches the format, carries the tool
  bootstrap and the manifest. The most heavily protected pages.
- **Human-first, machine-backed.** Readable text or source (checksummed line by line),
  anchored by one backing machine page holding its exact bytes. About twenty readable
  pages to one machine page.
- **Machine-only.** Binary data: a dense field plus a human header, no hand tier implied.

## The substrate (`call/0005`)

- One compressed stream, chunked into machine pages of at most 64 KiB. A file under 64 KiB
  is a one-page stream; a larger file spans pages in sequence; many files are one stream.
- A manifest in the preamble maps each file to its byte range, length, and SHA-256.
- Each machine page header: document id, page k of K, chunk c of C, byte offset, bytes on
  the page, chunk SHA-256. Sequencing reassembles deterministically; hashes catch a
  misordered or corrupt page.

## Integrity, layered

- Per line: Fletcher-16 (hand-checkable).
- Per page and per file: SHA-256 (machine).
- Within a page: Reed-Solomon erasure for smudges and cracks.
- Across pages: one parity page per group recovers a wholly lost page.

## Typesetting (`call/0006`)

Vendored open faces (TeX Gyre Pagella under the GUST licence for the body, Noto under the
OFL for the world's scripts, Noto Sans Mono for the checksums). CJK variant selection by a
BCP-47 tag per run. The tool emits data and codes; a Typst template typesets the PDF.

## Open, before the pipeline

- The physical print-damage-scan measurement (the erasure and substitution rates, the final
  correction split); see [poc-findings.md](poc-findings.md).
- The real Reed-Solomon codec, so a native field is genuinely decodable rather than texture.
- The preamble draft and the assumed-knowledge floor.
