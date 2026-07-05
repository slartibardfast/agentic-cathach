# The dense machine tier: 64 KiB per page, cathach-native

- Status: accepted
- Superseded in part: the finder stance (no finder patterns; three corner registration marks) is revised by call/0011.
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

The bulk payload needs a dense tier that a machine reads and that survives to the far
future. A standard QR code is scannable today, but it is a heavy bootstrap: rebuilding
its decoder from a printed spec means reconstructing the whole QR standard, and its
finder patterns cost about a third of the area. The tier also needs a density target so
the page can be designed around it.

## Decision

- Target 64 KiB of payload per A4 page. The binding and margins leave about 44,000 mm²
  of usable area, so a module of about 0.22 mm carries the target with one-third of the
  field given to Reed-Solomon erasure. That module is about five dots at 600 dpi, so the
  dense machine tier prints at 600 dpi. The human tier keeps its naked-eye size.
- The durable dense tier is cathach-native. It is a bilevel module field in the same
  base-N and Reed-Solomon family as the hand tier. A thin border and three corner
  registration marks frame it, and it has no finder patterns. Its decoder is the printed
  tool source, so a far-future rebuild teaches one algorithm across every tier.
- A QR code stays a convenience layer. It is scannable by a phone today. It is not a
  recovery dependency, so the format never has to teach the QR standard.

## Consequences

- One A4 page holds about 64 KiB, so a book of roughly 300 readable pages fits in about
  15 dense pages before compression, or 3 to 4 after.
- The tool implements one codec for the durable tier; QR is an optional extra encoder.
- The dense texture is high-entropy by nature, since coded data reads as random.
