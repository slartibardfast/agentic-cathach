# Trilingual preamble: English, Mandarin, Spanish

- Status: accepted
- Scope: cathach
- Date: 2026-07-04

## Context and Problem Statement

The preamble teaches a reader to decode the book, so it carries the same meaning in three
languages chosen to reach the largest literate population. English and Mandarin were
settled early. The third was open between Hindi and Spanish.

## Decision

The third language is Spanish, so the preamble languages are English, Mandarin, and
Spanish. The reasons:

- **Literate reach.** Spanish has about 519 million native speakers and between 558 and
  636 million in total, at a very high literacy (above ninety percent across its
  countries). Its readable population is at least as large as Hindi's and cleaner to
  state, since India's literacy is about 81 percent and lower still in the Hindi belt.
- **Geographic dispersal.** Spanish is official in twenty-one countries across four
  continents, while Hindi is concentrated in one region. A durable artifact wants its
  fallback readers spread out, so that losing one region does not lose the tier.
- **The preamble's job is reach, not script variety.** Spanish shares the Latin script
  with English; script diversity is a job for the payload and the samples, which already
  span many writing systems.

## Consequences

- The preamble is laid out in three parallel columns or passes, one per language.
- Font coverage needs no addition: the vendored set already covers Latin and Han.
- The choice rests on present figures. It is revisited only if the populations shift
  markedly.
