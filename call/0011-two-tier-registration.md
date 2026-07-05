# Two-tier distributed registration

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

A scanned page must be oriented and its grid pitch recovered before any module is read.
call/0004 gave the native dense field a thin border and three corner registration marks and
no finder patterns. The finder survey showed that stance to be fragile: corner-only finders
are a single point of failure, and coarse finders alone are what makes an implicit-grid
scheme fail in practice (prior-art.md, "Finder and registration robustness").

## Decision

Registration is two-tier:

- Error-correcting fiducial markers at the page corners and edges for global orientation
  and homography. Purpose-built markers of this kind carry a selectable Hamming distance and
  survive damage to a marker, so a small lattice still fixes orientation and warp when any
  one marker is lost.
- An interior mesh of dedicated timing marks refined to sub-pixel precision for local grid
  pitch and page curl. A distributed mesh resynchronizes locally, so damage to one mark
  stays regional, and the mesh absorbs paper stretch and curl.

The reason is a single-point-of-failure argument. Corner-only finders kill the whole read
when damage lands on the finder, which is the registration fragility that makes PaperBack
fail, and its finder patterns carry no error correction. A distributed mesh and
error-correcting markers each remove that failure mode.

This supersedes the finder stance of call/0004, its "a thin border and three corner
registration marks" and "no finder patterns". The rest of call/0004 stands: the density
target, the cathach-native dense tier, the QR-convenience layer, and the 600 dpi module
size.

## Consequences

- The dense field carries error-correcting fiducial markers and an interior timing mesh in
  place of three bare corner marks.
- The fiducial markers are larger than a module, about two to three millimetres, so they
  are found even when the dense field is degraded.
- The rest of call/0004 continues to govern the dense tier.
