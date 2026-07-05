# Redundancy and erasure-mode decoding

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

The machine tier needs an error-correction scheme matched to the physical failure model.
Damage to fused toner is spatially contiguous: cracking, flaking, and liquid rings take out
regions rather than scattered independent dots, and such damage announces its location. The
choice of code, its overhead, its decode mode, and its page layout were open
(format-proposal.md).

## Decision

The machine tier uses Reed-Solomon at roughly one-third overhead, decoded in erasure mode
from an image-derived per-module damage map. The reasoning is in prior-art.md and
format-proposal.md:

- Reed-Solomon over LDPC, because the failure model is bursty and Reed-Solomon beats LDPC
  on burst errors. One-third overhead sits at the safe end of the mainstream range.
- Erasure mode is the format's technical edge. A located erasure costs one unit of
  minimum distance where a blind error costs two, so the same parity budget recovers twice
  as many located erasures. Contiguous toner damage announces its location, and a per-module
  confidence map turns that announcement into erasure flags. The surveyed paper-backup tools
  mostly treat all damage as unlocated error and leave the two-to-one saving unclaimed.

Layout follows CIRC. A group's glyphs are never laid contiguously. The field is interleaved
so that the largest expected burst, measured in glyph widths and divided by the interleave
depth, stays within the group's recoverable-erasure count. A light inner check localizes
damage and flags erasures, and a de-interleaved outer group recovers from the flags.

## Consequences

- Decoding depends on an image damage map, so the scanner tier produces a per-module
  confidence value, not just a bit.
- The interleave depth and the split between Reed-Solomon and repetition stay gated on the
  channel measurement, since both the worst-case burst width and the achievable interleave
  depth come from it.
- The guard count for the hand tier follows the same measurement, through the
  erasures-plus-one rule in call/0009.
