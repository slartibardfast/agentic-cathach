# Readable text as the front modality; the exact-byte tiers behind

- Status: accepted
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

`call/0002` set three exact-byte modalities (see that record). For a text payload,
though, the most accessible representation is the text itself: a reader who knows the
script needs no decoding at all. Unicode, the sample corpus, is text, so this is the
common case rather than an edge one.

## Decision

Add a readable-text carve as the front modality. When the payload is valid UTF-8 text,
print the text itself, set to be read directly, with each line carrying a Fletcher-16
gloss in the right margin (a modern masorah) so a reader can confirm the line against the
original. The exact-byte tiers from `call/0002` sit behind it, for recovery when the text
is damaged and for payloads that are binary. Genuinely binary content has no readable
form and falls back to the armor.

## Consequences

- A reader meets the text first; the exact-byte tiers recover the bytes when the reading
  is in doubt or the payload is not text.
- The per-line gloss localises a misread to its own line, the cheap located-erasure
  regime, so the human tier repairs it by subtraction.
- Cormac adds the readable pass and the text-or-binary decision to the encoder; the
  earlier packing experiment (dense binary into Unicode codepoints) is dropped.
