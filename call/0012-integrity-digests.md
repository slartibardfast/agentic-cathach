# Integrity digests, one per tier

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

The book needs a fixity claim a reader can check and a rebuilder can verify against. The
hand tier cannot compute a cryptographic hash on paper, and the machine tier should carry
one. The design fixed a two-tier integrity constraint but left the digests unspecified. The
rebuilt-tool tier checks a rebuild against exactly these digests, so both must be defined
and printed (format-proposal.md).

## Decision

Two digests, both printed, each per file and for the whole book.

The hand tier is a two-digit running pair in the payload base, computed over the data
digits of the covered span in reading order, with the guards excluded. Two accumulators A
and B start at zero. For each data digit, first add the digit to A and keep the ones place,
then add A to B and keep the ones place. Print A then B. This needs addition and the taught
set-aside-full-groups step only, no multiplication and no division, so it stays inside the
hand budget. The second accumulator makes the pair order-sensitive, so a transposition moves
B even when A is unchanged. Its strength is an honest transcription and tamper tripwire, not
cryptography, and the spec states this. Excluding the guards keeps repair and verification
as independent evidence, since a repaired row re-checks against the same printed digest.

The machine tier is SHA-256 over the exact bytes, printed as concrete digits and confirmed
by a rebuilt tool in a moment. No human computes it.

## Consequences

- The table of contents carries both digests, per file and for the whole book.
- A rebuilder checks a rebuild against the printed SHA-256, and a hand reader confirms a
  span against the printed running pair.
- The hand digest is scoped honestly as a tripwire, not a fixity guarantee at the strength
  of the machine digest.
