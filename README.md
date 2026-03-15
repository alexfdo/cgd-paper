# CGD-paper: Controlled Generative Development

**A Deterministic Structural Core for AI-Native Software Design**

Alexander Fedorenko | Independent Researcher | 2026

## Contents

- `paper/cgd_paper.pdf` — compiled PDF of the manuscript
- `paper/references.bib` — BibTeX (16 sources)
- `appendices/appendix-a-yasf.md` — Appendix A: YASF specification
- `appendices/appendix-b-graph-v03.md` — Appendix B: Graph model v0.3
- `appendices/appendix-c-tables-v02.md` — Appendix C: Tables v0.2
- `appendices/appendix-d-json-schemas.md` — Appendix D: JSON Schema descriptions
- `appendices/appendix-e-examples.md` — Appendix E: End-to-end examples
- `schemas/graph.schema.json` — JSON Schema for the graph model
- `schemas/tables.schema.json` — JSON Schema for table signatures
- `examples/graph.json` — Canonical graph example (user registration)
- `examples/tables.json` — Canonical table signatures example

## About

CGD is a **contract-first** framework for AI-assisted development. The specification — expressed as a formal triple **T = ⟨Y, G, Σ⟩** — is compiled, verified for contractual compatibility, and only then passed to the code generator.

## Key concepts

- **Y** — YASF specification in controlled natural language
- **G** — Typed directed graph of functions and tables
- **Σ** — Table signatures (fields, keys, kind, constraints)
- **C(f) = ⟨Cin, Cout, R, W, Tin, Tout⟩** — Six-component function contract
- **L0–L4** — Five-level infrastructure for layered defect filtering

## Status

- Manuscript is in clean submission state (English).
- References are integrated (16 sources with BibTeX).
- JSON artifacts are synchronized with the formal core.
- Appendices A–E translated and synchronized.
- Reference implementation is on the roadmap.

## License

CC BY 4.0
