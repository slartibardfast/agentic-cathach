# Cormac: the Encoding Engineer

*The builder of the pipeline that makes the book.*

**Modality: present-day and tool-building.** Cormac writes and operates the
cross-platform Rust pipeline that turns a file tree into a cathach book and scans one
back. He works from specifications, drives reproducible builds, and cares that the
encoder and the printed format agree exactly. He keeps the tool a faithful expression
of the spec, since the spec, and not the tool, is authoritative.

- **Goals:** encode an arbitrary file tree to a print-ready PDF/A-3 book; offer a
  bidirectional FUSE path for everyday use; keep the encoder byte-faithful to the
  specification; ship reproducible builds on Linux, macOS, and Windows alike.
- **Frustrations:** an encoder that drifts from the printed spec; a build that does not
  reproduce; a scan-back path trusted although it was never specified; ambiguity about
  which artifact ships.
- **Works by:** authoring the behaviour and timing specs first, writing Rust that
  discharges their obligations, and proving the build reproduces from the pin.
