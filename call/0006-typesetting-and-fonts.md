# Typesetting and fonts: vendored open faces, CJK variant selection, Typst

- Status: accepted
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

The book must be beautiful and reproducible, and it must render the world's scripts. A
durable archival tool cannot depend on whatever fonts an operating system happens to
carry, and it cannot ship fonts it may not redistribute. Han unification also means one
codepoint has several national forms, so a renderer must be told which one to draw.

## Decision

- Body face: Palatino Linotype where an operator already has it. The vendored fallback is
  TeX Gyre Pagella (GUST Font License, an instance of the LPPL), which is metric-compatible
  with Palatino and redistributable.
- World scripts: the Noto family under the SIL Open Font License. The vendored set is Noto
  Serif for Latin, Greek, and Cyrillic; Noto Serif CJK per region; Noto Serif Devanagari;
  Noto Naskh Arabic; Noto Serif Hebrew; a monochrome Noto Emoji for the black-and-white
  target; and Noto Sans Mono for the checksums. Every vendored face is redistributable, so
  the build is deterministic and offline.
- CJK variant selection is first class. Each text run carries a BCP-47 language tag, so a
  Han codepoint renders in its national form, and this pairs with one language per line.
- The review pipeline: the tool emits data as JSON and codes as SVG and PNG, and a Typst
  template typesets the PDF. Drawing is left to the typesetter, never hand-rolled.

## Consequences

- Typesetting is reproducible and redistributable; production subsets the CJK faces to the
  glyphs a document actually uses, which shrinks the vendored size to a few kilobytes; the
  full faces run into tens of megabytes.
- The trilingual preamble and the multi-script payload render from one vendored font set.
- The licence texts (GUST and OFL) ship beside the fonts.
