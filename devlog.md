# replicating-sutra-tensor-op-rnns-as-a-compilation-target — Devlog

**This file is where "done" lives.** `queue.md` is delete-only: when a queue
item is finished, the item is **deleted from `queue.md`** and a dated entry
is **appended here**, in the same commit as the work, then pushed. Never
tick a box in place — a checked box left in `queue.md` is the failure mode
this file exists to prevent.

Also record releases (tag + a one-line note), notable milestones, and
anything else worth a chronological trail. Newest entries at the bottom.

This is the **same convention as the cleanvibe repo's own `devlog.md`** —
every cleanvibe-scaffolded project gets one for the same reason.

See `CLAUDE.md` § "Workflow Rules" and `queue.md`'s preamble.

---

## 2026-05-22 — Project scaffolded

Scaffolded with `cleanvibe new` (cleanvibe v1.5.0). Future entries
land here as queue items get deleted.

## 2026-05-22 — Source acquired

`python download_paper.py` fetched arXiv:2605.20919v1: extracted the LaTeX
e-print to `replication_target/source/` (committed) and the PDF (gitignored).
The substantive body is `paper.tex.body`.

## 2026-05-22 — Recipe found + downloaded (recipe-first path confirmed)

The paper's §Reproducibility ships a runnable recipe two ways: a self-
contained replication package
(`sutra.emmaleonhart.com/sutra-replication-package.zip`, downloaded and
extracted to `replication/` — zip gitignored, contents committed) and an
upstream repo `github.com/EmmaLeonhart/Sutra` (fallback). The package ships
`SKILL.md` (agent-runnable), the compiler SDK, 26 `.su` examples, and the §3
experiment scripts with reference `*_results.json`. No from-scratch
reimplementation needed.

## 2026-05-22 — Went live: public repo + CI

Created public repo
`github.com/EmmaLeonhart/replicating-sutra-tensor-op-rnns-as-a-compilation-target`
and pushed. Added `replicate.yml` (offline numpy-only synthetic-capacity
check) — green on the first cloud run (`capacity_synth: ok`, ollama
unavailable). Enabled GitHub Pages (Source: GitHub Actions); site at
`emmaleonhart.github.io/replicating-sutra-tensor-op-rnns-as-a-compilation-target`.

## 2026-05-22 — Recipe run: compiler + capacity claims reproduced

Ran the `SKILL.md` recipe. Reproduced (this machine): 10-program smoke test
PASS; full compiler suite **363 passed, 8 skipped** (paper said 237/7 — suite
grew); loops 23/23; codebook 7/7 (after building the Rust FFI); torch.compile
3/3; T-budget invariance stable. **§3.1 capacity table reproduced exactly on
all four substrates** (nomic/all-minilm/mxbai LLMs + ESM-2 protein LM); §3.1.1
crosstalk reproduces the decay (chain=1 100% → chain=8 chance). References
checked — all real, load-bearing ones say what the paper claims.
