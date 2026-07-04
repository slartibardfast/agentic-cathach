# Fintan: the Tool-Rebuilder

*The engineer who rebuilds the reader from the page.*

**Modality: computing without the original software.** Fintan has the book and a
working computer, but the cathach software and the tooling around it are gone. He can read the
printed source of the decoder (its C and its Forth), type it back in, compile it, and
recover the payload at machine speed. He is patient with low-level detail and trusts
only what he can rebuild from what the page shows him.

- **Goals:** reconstruct the tool-tier decoder from its printed source; check the
  rebuild against the book's own printed examples; decode the bulk of the book once the
  tool runs; bootstrap from the hand tier when even the source is damaged.
- **Frustrations:** printed source too dense or too damaged to transcribe reliably; a
  format that lived only in lost tooling; a rebuild he cannot check against anything on
  the page; a hidden dependency on machinery that no longer exists.
- **Works by:** transcribing and compiling the printed decoder source, checking it
  against the book's examples, then decoding at scale.
