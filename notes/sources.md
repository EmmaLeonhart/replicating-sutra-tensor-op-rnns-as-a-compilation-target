# Sources

## Paper
- arXiv:2605.20919v1 — "Sutra: Tensor-Op RNNs as a Compilation Target for
  Vector Symbolic Architectures", Emma Leonhart, 2026-05-20.
- LaTeX e-print source extracted to `replication_target/source/`. The
  substantive body is `paper.tex.body` (pandoc-generated from the authors'
  `paper.md`); `paper.tex` is just the NeurIPS-2026 wrapper.

## Reproduction recipe — FOUND (recipe-first path applies)
The paper's §Reproducibility (paper.tex.body L1003–1016) ships a runnable
recipe two ways:

1. **Self-contained replication package** —
   `https://sutra.emmaleonhart.com/sutra-replication-package.zip`
   (downloaded; the `.zip` is gitignored, its extracted contents committed
   under `replication/sutra-replication-package/`). Ships:
   - `SKILL.md` — agent-runnable recipe: shell blocks, each asserting a
     paper success condition (non-zero exit = claim does not reproduce).
   - `REPRODUCE.md` — paper-section → command map.
   - `SYNTAX.md` — language reference.
   - `sdk/sutra-compiler/` — the compiler (pure Python) + 363-test suite.
   - `examples/` — 26 `.su` programs + `_smoke_test.py` (10-program corpus).
   - `experiments/` — §3 reproduction scripts + reference `*_results.json`.
   - `sutraDB/` — Rust crates for the embedded-codebook FFI (optional).
2. **Upstream repo** — `https://github.com/EmmaLeonhart/Sutra` (public;
   pushed 2026-05-22). README calls it a *fallback*, not the primary path;
   the bundled package is self-contained.

Decision: **run the bundled `SKILL.md` recipe** (recipe-first). No
from-scratch reimplementation needed — the recipe covers every headline
claim. `scripts/run.py` orchestrates the recipe and records reproduced-vs-
reported into `results/`.

## How much of the paper the recipe covers
The recipe reproduces **every empirical claim** in the paper:
- §3.1 capacity (rotation vs. Hadamard) across 3 LLM substrates + ESM-2.
- §3.1.1 chained-bind crosstalk depth.
- §3.4 first-class loops (soft-halt RNN cells), §3.5 embedded codebook.
- §3.6 differentiable training through the compiled graph.
- §3.7 trained scalar gain baked back into recompilable `.su`.
- §4 compiler pipeline (full pytest suite), §5 demonstration corpus.

Nothing in the headline claims is left for a from-scratch fill. The
replication work is therefore **verification of the recipe's output against
the paper's reported numbers**, plus orchestration + a findings write-up.

## Environment actually used (this machine)
- Windows 11; `python` → Python 3.13.3 with torch 2.10.0+cu128 (CUDA),
  pytest 9.0.2. (Paper asks 3.11+; 3.13 works.)
- Ollama 0.17.1 local; pulled `nomic-embed-text`, `all-minilm`,
  `mxbai-embed-large`.
- Rust/cargo 1.94.0 — built `sutra-ffi` (`sutra_ffi.dll`) so the embedded-
  codebook tests run instead of skipping.
