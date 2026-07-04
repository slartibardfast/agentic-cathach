# Document architecture: page types, the human-backed-by-machine ensemble, the stream substrate

- Status: accepted
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

cathach preserves a file tree of text, source, and binary data, recoverable at several
tiers and beautiful to hold. The human tier is about twenty times sparser than the dense
machine tier (see `call/0004`), so the bulk cannot be rendered by hand. The format also
has to handle a file of any size, from a few bytes to far more than one page holds.

## Decision

Three kinds of page:

- **Preamble.** Sparse, every tier, fully hand-recoverable. It teaches the format and holds
  the tool bootstrap and the manifest. Multi-modal redundancy belongs here.
- **Human-first, machine-backed.** Text or source set to be read, each line carrying its
  Fletcher-16 checksum gloss so a reader can verify and transcribe it, anchored by one
  backing machine page that holds the section's exact bytes. About twenty readable pages
  to one machine page.
- **Machine-only.** Binary data has no readable form, so a dense field plus a human header
  describes it. No hand tier is implied on such a page.

One substrate under all of it:

- The whole payload is one compressed stream, chunked into machine pages of at most 64 KiB.
  A file under 64 KiB is a one-page stream; a file over 64 KiB spans several pages in
  sequence; many files are still one stream. There is no special case for large files.
- A manifest in the preamble maps each file to its byte range in the stream, its length,
  and its SHA-256.
- Each machine page header carries the document id, the page number k of K, the chunk
  number c of C, the byte offset, the bytes on the page, and the chunk SHA-256. Sequencing
  makes the pages reassemble deterministically, and the hashes catch a misordered or
  corrupt page.

Whole-page loss: one parity page per group recovers a lost page from its siblings, above
the within-page erasure coding.

## Consequences

- The book reads as a human document; the exact bytes ride underneath it.
- A small file and a large file use the same mechanism; only the page count differs.
- Integrity is layered: per-line Fletcher-16 for the hand, per-page and per-file SHA-256
  for the machine, within-page Reed-Solomon erasure for smudges, and cross-page parity for
  a lost page.
- The manifest and the preamble are the single point a payload page depends on, so they
  are the most heavily protected part of the book.
