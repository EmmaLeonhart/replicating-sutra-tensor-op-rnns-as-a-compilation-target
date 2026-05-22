# Replicating: Sutra: Tensor-Op RNNs as a Compilation Target for Vector Symbolic Architectures

**arXiv:** [2605.20919](https://arxiv.org/pdf/2605.20919v1) - **HTML:** [2605.20919](https://arxiv.org/html/2605.20919v1)
**Authors:** Emma Leonhart
**Published:** 2026-05-20T09:04:36Z

## Abstract

Sutra is a typed, purely functional programming language whose compiled forward pass is a PyTorch neural network. The compiler beta-reduces the whole program -- primitives, control flow, string I/O -- to one fused tensor-op graph over a frozen embedding substrate. Rotation binding, unbind, bundle, polynomial Kleene three-valued logic, and tail-recursive loops all lower to tensor operations; the Kleene connectives are Lagrange-interpolated polynomials exact on the {-1, 0, +1} truth grid. Validation is one fact tested two ways. (1) The same program runs on four frozen embeddings spanning two modalities -- three text encoders (nomic-embed-text, all-minilm, mxbai-embed-large) and one protein language model (ESM-2) -- and decodes bundles at 100% accuracy through width k=8 on every substrate, where the textbook Hadamard product has already collapsed (2.5% on mxbai-embed-large, 7.5% on all-minilm). (2) PyTorch autograd flows through the actually compiled graph: a fuzzy-rule classifier written in .su trains from random init (18.7 +/- 9.5%; chance = 20%, five classes) to 100.0 +/- 0.0% (three seeds) by backpropagating through the emitted graph, the symbolic source unmodified. A weighted variant additionally trains a scalar cosine gain and writes it back into the .su source as a numeric literal; recompiling reproduces the trained behaviour to ~2e-7 per logit, so the trained model is itself legible, recompilable code. The same artifact is therefore both a logic program and a trainable neural network.

## Replication status

Not started. The agent-executable plan is in [`SKILL.md`](./SKILL.md);
the concrete step queue is in [`queue.md`](./queue.md). The efficient path is
**recipe-first**: get the LaTeX source, find and run the authors' reproduction
recipe (often shipped right in the paper), then verify it against the paper and
fill only the gaps.

## What this repo produces

Three compounding artifacts:

1. **The replication** — runnable code under `src/` + `scripts/run.py`.
2. **The legibility layer** — `FINDINGS.md`, published as a GitHub Pages
   site with a transportable PDF report (built by GitHub Actions).
3. **`SKILL.md`** — a reusable, agent-executable replication methodology.

## Layout

- `replication_target/` — the paper and everything pulled about it:
  - `source/` — extracted arXiv LaTeX/e-print source (committed; the primary,
    token-efficient text — read the `.tex` directly). Fetched by
    `python download_paper.py`; the raw archive is gitignored.
  - `paper.pdf` — downloaded PDF (gitignored; fallback / complete record).
  - the authors' code, if any, as a git **submodule**.
- `replication_skill.md` — the authors' recipe, if one is shipped (run first).
- `data_lake/` — other downloaded/supplied material (NOT the paper).
- `src/` — your reimplementation. `scripts/run.py` — CI entry point.
- `results/` — metrics JSON (gitignored). `FINDINGS.md` — the report.
- `paper.json` — frozen metadata pulled from the arXiv API.
- `.github/workflows/` — `pages.yml` (site + PDF), `package.yml` (ZIP).

## Deliverables (GitHub Actions)

To publish, **make this repo public** and set **Settings -> Pages -> Source:
GitHub Actions**. Then `pages.yml` deploys the findings site + PDF report and
`package.yml` builds a downloadable ZIP replication package. Site shape
inspiration: http://sutra.emmaleonhart.com/
