# Preamble pedagogy and the floor reconciliation

- Status: accepted
- Scope: cathach
- Date: 2026-07-05

## Context and Problem Statement

The preamble teaches a reader to decode the book by hand from a low floor,
language-independently. call/0008 fixed the assumed-knowledge floor at counting, yet its
budget names addition and multiplication, and no draft taught multiplication before the
weighted guard used it. The teaching survey converged on a set of idioms that close this
gap and lift the teaching off any one language's grammar of number
(prior-art.md, "Self-describing messages and teaching a naive reader"; format-proposal.md).

## Decision

The preamble follows this teaching order and set of idioms:

- Teach multiplication as arrays and repeated equal groups, before any figure uses it. The
  weight is then presented as copying each column's digit as many times as the strokes
  beneath it and adding. Every weight is small and every digit less than the base, so the
  arrays are tiny and fully drawable.
- Mark the units position explicitly, so reading direction and place value are fixed by the
  figure rather than inferred.
- Show each value in more than one form at once, so notation is welded to quantity and the
  three languages share one figure.
- Teach each new symbol by many worked instances, never by a figure that needs an implied
  verb.
- Place a built-in decode self-test before the guard machinery, so a reader who parsed the
  numerals correctly reproduces a known figure and a wrong parse visibly fails.
- Keep the group-size-is-a-choice figure, so the base is taught as one arbitrary choice.
- Teach length-based unpadding, so a reader keeps only as many values as the recorded
  length and reads the saltire as the end-of-data mark (call/0009).

This amends call/0008. The assumed-knowledge floor stays at counting. The arithmetic named
in that decision's budget, addition and multiplication, is taught from that floor rather
than presumed of the reader. This record clarifies call/0008 without superseding it.

## Consequences

- The preamble builds multiplication before the weighted guard, which closes the floor
  breach.
- The floor named in call/0008 is unchanged; only the status of its budget's arithmetic is
  clarified as taught.
- The concrete figures are regenerated from the settled spec once the alphabet is fixed.
