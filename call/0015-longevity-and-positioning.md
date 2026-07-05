# Longevity and honest positioning

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

A durable-media project invites an overstated longevity claim and an overstated novelty
claim. The medium is paper and fused laser toner, not etched metal, and much of the coding
core is well-trodden prior art. The positioning needs to be honest so the specification and
any external claim do not drift apart (prior-art.md, "Long-term archival projects and
media"; format-proposal.md).

## Decision

The longevity claim is centuries on ISO 9706 permanent paper with monochrome toner,
dark-stored, recoverable with no special hardware and trivially copyable. It is not a
metal-etch millennium. The substrate, not the toner, is the limit, since the toner is inert
and the paper governs the ceiling.

The genuine novelty is stated as: the unaided pen-and-paper hand tier, the self-describing
preamble on commodity print, the bound-book form, and erasure-mode decoding against a named
physical failure model. The coding core, Reed-Solomon, interleaving, and finder patterns, is
prior art to reuse, not to reinvent.

## Consequences

- Any external claim about longevity states centuries on permanent paper, not a metal-etch
  timescale.
- Design effort goes to the differentiated parts, and the coding core reuses proven
  constructions.
- The novelty statement holds the specification and any published description to the same
  honest scope.
