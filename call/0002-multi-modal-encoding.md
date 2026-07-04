# Multi-modal encoding: digit, ASCII, and QR tiers

- Status: accepted
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

Gate 1 of the POC review, the payload alphabet, is resolved: the human tier uses
Hindu-Arabic digits, which every literate person already reads and which keeps the
assumed-knowledge floor low. The cast then needs recovery at several capability tiers,
and no single representation serves all of them.

## Decision

Carry the same payload bytes in three parallel modalities on the page, each an
independent recovery path.

1. Digit tier. Base-N digit groups with the hand-computable checks S and W. It fills a
   located erasure by subtraction, with no machine and no division. Serves Oisín.
2. ASCII carve. A pure-text Base32 block with a length and a hand-checkable digit,
   readable through any text-only channel and transcribable by hand. Serves Fintan.
3. QR tier. A standard QR code, scannable by any phone camera with no app. Serves
   Aoife, and gives Bríd a quick bulk verification.

Each modality decodes the same bytes on its own. Bríd, the primary persona, gains three
paths that fail independently. The archival promise rests on the digit and ASCII tiers;
the QR is the convenient scan, since a QR's longevity is lower than the printed digits'.

## Consequences

- Page overhead rises, because the bytes are written three times, but each writing serves
  a distinct persona and tier, and the redundancy is across independent formats.
- Cormac implements three encoders in the Rust pipeline, each with its own behaviour
  obligations in the spec.
- The POC demonstrates all three from one payload (`software/cathach/poc/`, output
  `out/multimodal.txt`).
- Gates still open: radix and overhead, the preamble typefaces, and the physical
  measurement. See `plan/0001-durable-book-format/poc-findings.md`.
