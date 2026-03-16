Понял. Сначала определяю новую структуру проекта на GitHub, потом переписываю README.

**Новая структура репозитория `cgd-paper`:**

```
cgd-paper/
├── README.md
├── LICENSE
├── paper/
│   ├── cgd-paper.tex
│   ├── cgd-paper.pdf
│   ├── references.bib
│   ├── appendix-a.tex
│   ├── appendix-b.tex
│   ├── appendix-c.tex
│   ├── appendix-d.tex
│   └── appendix-f.tex
├── figures/
│   ├── demo1-structured.pdf
│   ├── demo2-no-constraint.pdf
│   └── demo2-top-level.pdf
├── appendices-md/
│   ├── appendix-a-cgd-response-standard-v1.md
│   ├── appendix-b-execution-profile-v1.md
│   ├── appendix-c-compute-subgraph-contract.md
│   ├── appendix-d-task-driven-context-assembly.md
│   ├── appendix-e-demo-artifacts.md
│   └── appendix-f-yasf-specification.md
├── prompts/
│   ├── scenario-01-naive.md
│   ├── scenario-02-standard.md
│   └── scenario-02-top-level.md
├── runs/
│   ├── scenario-01/
│   │   └── claude-opus-4.6.md
│   ├── scenario-02/
│   │   └── gpt-5.4-thinking.md
│   └── scenario-02-top-level/
│       └── gpt-5.4-thinking.md
└── drafts-ru/
    ├── cgd-paper-ru.md
    └── appendices-ru/
        ├── appendix-a.md
        ├── appendix-b.md
        ├── appendix-c.md
        ├── appendix-d.md
        ├── appendix-e.md
        └── appendix-f.md
```

**Новый README.md:**

```markdown
# CGD Paper: Controlled Generative Development

**From Intent to Structural Realization and Preparation for Controlled Execution**

Alexander Fedorenko · Independent Researcher · 2026

---

## About

CGD (Controlled Generative Development) is a contract-first framework
for AI-native software design. The core idea: before any code is generated,
the model output must be captured as a verifiable structural realization
where incompleteness is observable, not hidden.

The architecture is built around the pair **D = ⟨I, R⟩**, where:

- **I** — intent representation (expressed via YASF, a controlled natural language)
- **R = ⟨G, Σ, C⟩** — structural realization:
  - **G** — typed directed graph of functions and tables
  - **Σ** — table signatures (fields, keys, kind, constraints)
  - **C(f) = ⟨Cin, Cout, R, W, Tin, Tout⟩** — six-component function contracts

Three control loops govern the process: **output contract** (what the model
must produce), **abstraction control** (at what level of detail), and
**verification profile** (what properties are checked before code generation).

## Repository structure

```
paper/              LaTeX source, compiled PDF, references.bib, appendix .tex files
figures/            Mermaid-exported diagrams (PDF)
appendices-md/      Appendices A–F in Markdown (source of truth)
prompts/            Prompt files used in demo scenarios
runs/               Full model outputs (demo artifacts)
drafts-ru/          Russian draft of the paper and appendices
```

## Paper

The compiled manuscript is at [`paper/cgd-paper.pdf`](paper/cgd-paper.pdf).

| Section | Topic |
|---------|-------|
| §1 | Introduction |
| §2 | Three control loops: output contract, abstraction control, verification profile |
| §3 | Positioning among spec-first tools |
| §4 | Architectural framing: D = ⟨I, R⟩ |
| §5 | The formal core: R = ⟨G, Σ, C⟩ |
| §6 | Verification-driven dialogue and specification closure |
| §7 | Task-driven context assembly |
| §8 | Demo 1 — controlling form |
| §9 | Demo 2 — controlling abstraction |
| §10 | Demo 3 — verification-driven correction |
| §11 | Limitations and threats to validity |
| §12 | Future work |
| §13 | Conclusion |

## Appendices

| Appendix | File | Content |
|----------|------|---------|
| A | `appendices-md/appendix-a-cgd-response-standard-v1.md` | CGD Response Standard v1.3 |
| B | `appendices-md/appendix-b-execution-profile-v1.md` | Execution Profile |
| C | `appendices-md/appendix-c-compute-subgraph-contract.md` | ComputeSubgraphContract algorithm |
| D | `appendices-md/appendix-d-task-driven-context-assembly.md` | Task-driven context assembly |
| E | `appendices-md/appendix-e-demo-artifacts.md` | Demo run artifact index |
| F | `appendices-md/appendix-f-yasf-specification.md` | YASF specification |

## Demo artifacts

| Demo | Model | Run file |
|------|-------|----------|
| Demo 1 (baseline) | Claude Opus 4.6 | `runs/scenario-01/claude-opus-4.6.md` |
| Demo 1 (structured) | GPT-5.4 | `runs/scenario-02/gpt-5.4-thinking.md` |
| Demo 2 (top-level) | GPT-5.4 | `runs/scenario-02-top-level/gpt-5.4-thinking.md` |

## References

1. OpenAI. *Symphony*. 2026. [github.com/openai/symphony](https://github.com/openai/symphony)
2. Piskala D. B. *Spec-Driven Development*. arXiv:2602.00180, 2026.
3. Breslav A. *CodeSpeak*. 2026. [codespeak.dev](https://codespeak.dev/)
4. Augment Code. *Build with Intent*. 2026. [augmentcode.com/product/intent](https://www.augmentcode.com/product/intent)
5. Cursor. *Rules Documentation*. 2026. [cursor.com/docs/rules](https://cursor.com/docs/rules)
6. Anthropic. *Best Practices for Claude Code*. 2026. [code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)
7. GitHub. *Spec-Driven Development with AI*. 2025. [GitHub Blog](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)

## Status

- English manuscript: clean, compiled, ready for submission.
- Russian draft: finalized, stored in `drafts-ru/`.
- Appendices A–F: synchronized with the paper.
- Demo artifacts: three runs included.
- Reference implementation: on the roadmap.

## License

CC BY 4.0
