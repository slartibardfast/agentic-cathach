# Cast: the project's Who

Personas: hypothetical archetypal actors, grounded in research, that keep the work
anchored in who it serves. The process comes from Powell, Keenan and McDaid (2007),
which builds on the personas in Cooper and Reimann's Interaction Design. See
[applying-personas.md](applying-personas.md) for the process.

cathach serves six personas. Five cover the three decoding tiers and the people who make
and enjoy the books; the sixth is the specialist consulted on the preamble's
cross-linguistic soundness:

- [Bríd](brid-archivist.md), the archivist-preservationist (**primary**): decides
  what to preserve and stakes the institution's credibility on its survival.
- [Oisín](oisin-hand-decoder.md), the far-future hand-decoder: recovers a region with
  pen, paper, and the preamble alone.
- [Fintan](fintan-tool-rebuilder.md), the tool-rebuilder: rebuilds the decoder from the
  printed source once the software is gone.
- [Cormac](cormac-encoding-engineer.md), the encoding engineer: builds and operates the
  cross-platform Rust pipeline.
- [Aoife](aoife-casual-reader.md), the casual reader: points a phone camera at a
  coffee-table book and sees what it holds.
- [Nuala](nuala-numeral-linguist.md), the comparative linguist of number: checks that
  the preamble teaches mathematics from a near-universal floor, not from one language's
  habits.

Bríd is the primary persona: the customer who must be able to adopt, run, and defend
cathach, and whom a workflow designed for any of the others would not satisfy. She takes
highest priority in plan prioritisation. Aoife's phone-and-browser path is the standing
reminder that the machine tier must reach an unequipped reader, not only a dedicated one.
Nuala is not a reader of a finished book but the specialist who keeps the trilingual
preamble sound; her review feeds the language choice (`call/0007`), the assumed-knowledge
floor (`call/0008`), and the preamble draft under `plan/0001`.

These were built by discussion with the operator (2026-07-04). The persona goals and
scenarios feed the milestone stories in `plan/` and the acceptance criteria in the specs.
