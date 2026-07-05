# Print resolution

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

The print resolution was stated two ways. The milestone README locked a 300 DPI print
target, while call/0004 and the architecture stub put the dense machine tier at 600 dpi with
a 0.22 mm module. A single target is needed so the page is designed once (format-proposal.md).

## Decision

The print resolution is 600 dpi, which resolves the contradiction in favour of call/0004.
The milestone README's 300 DPI lock is superseded by this decision.

The two visual registers print at their own scales under the one resolution:

- The dense payload register prints at 600 dpi with roughly 0.22 mm modules. A module of
  that size is about five scanner pixels, which a flatbed reads natively but a phone camera
  reads only marginally on a curved page, so the dense tier reads flatbed-primary.
- The plain instructional register keeps its naked-eye size.

## Consequences

- The dense tier is designed around a flatbed scan. The phone path wants larger modules or
  several fused captures, a trade for the alphabet and layout design rather than a new
  decision.
- The milestone README is updated to reflect the 600 dpi lock, since its 300 DPI figure is
  superseded here.
