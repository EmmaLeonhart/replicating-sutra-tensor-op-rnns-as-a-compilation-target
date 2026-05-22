# replicating-sutra-tensor-op-rnns-as-a-compilation-target - Work Queue

**This file is a queue of concrete, executable steps, not a state snapshot.**
Finished work lives in `devlog.md` (dated entries) and `git log`;
longer-horizon items live in `todo.md`. **When an item is done, delete it
from this file AND append a dated entry to `devlog.md` in the same commit,
then push.** No checkmarks, no status indicators in place.

**Why this file exists:** the replication plan is written here BEFORE
execution so an interrupted session resumes from the queue, not from chat.
The canonical methodology is `SKILL.md`; this queue is its executable form.

---

## Active — Replicate "Sutra: Tensor-Op RNNs as a Compilation Target for Vector Symbolic Architectures" (arXiv:2605.20919)

The scaffold already made commit 1 (the framework). The efficient path is:
get the LaTeX source, **find and run the authors' reproduction recipe FIRST**,
then verify its output against the paper and fill only the gaps. From-scratch
reimplementation is the fallback, not the default. Work top to bottom; delete
each item in the same commit that completes it (and append to `devlog.md`).

1. **Get the LaTeX source and commit it.** Run `python download_paper.py`. It
   downloads the arXiv **e-print source** (https://arxiv.org/src/2605.20919v1) — far cheaper to read than
   the rendered HTML, which embeds figures as huge base64 blobs — extracts it to
   `replication_target/source/`, and saves the PDF as a fallback record. The
   archive and PDF are gitignored; the extracted `source/` is committed. Read
   the paper straight from the `.tex` in `source/` — no HTML→markdown step. If
   the paper is PDF-only (no source), fall back to the PDF. Commit the extracted
   source.

2. **Create the GitHub repo and push — now, not at the end.** Create a PUBLIC
   repo and push: `gh repo create --public --source=. --push` (public is
   required for free GitHub Pages). From here on every commit pushes, so CI and
   Pages build as you go. (This is the step the v1.4.0 flow missed — the
   replication ran entirely locally and never went live.)

3. **FIRST, before any deep analysis: find the reproduction recipe in the
   source.** This is the highest-leverage step and it comes before reading the
   whole paper. Authors very often ship a recipe right in the e-print source —
   usually near the end of the paper: a `SKILL.md` / `AGENTS.md`, a
   `reproduce.*` / `replicate.*` / `run.sh` script, a `Makefile` reproduce
   target, a Dockerfile, or a **replication zip** referenced in the text.
   `download_paper.py` prints candidate files; also grep the `.tex` in `source/`
   for "reproduc", "replicat", "skill", "github.com", and asset/zip URLs.
   - Found a **skill/recipe file** → copy it to the repo root as
     `replication_skill.md` and commit.
   - Found a **replication zip** (in the source or linked in the paper) →
     download/extract it into `replication/` (add the zip to `.gitignore`,
     commit the extracted contents).
   - Found the **authors' code repo** → add it as a git submodule under
     `replication_target/` and record the decision in `notes/sources.md`.
   - Found nothing → note that in `notes/sources.md`; the rest of the queue is
     your from-scratch path.

4. **If a recipe exists, RUN IT FIRST and let it drive the rest.** Set up just
   enough environment to execute it, run it, and capture its output into
   `results/`. Then read the paper and assess **how much of the headline claims
   the recipe's output actually reproduces** — which numbers/figures it covers
   and which it doesn't. Record this in `notes/sources.md`. With a working
   recipe, most of what follows is *verifying its output against the paper*, not
   reimplementing from scratch. Commit.

5. **Check ALL references — always, recipe or not.** Walk the bibliography and
   confirm the key cited results / datasets / baselines the paper leans on
   actually say what the paper claims. This runs in every replication. Record
   anything load-bearing or surprising in `notes/claims.md`. Commit.

6. **Record `notes/claims.md`** — scoped to whatever the recipe did NOT already
   cover: headline claim(s); datasets (version/hash, where they live);
   models/methods in re-implementable detail; evaluation metrics and the exact
   reported numbers; compute envelope (GPU type, hours, memory — decides if CI
   can auto-run it). If the recipe covered everything, this is a short
   confirmation. Commit.

7. **Reimplement only the uncovered claims** under `src/` (skip anything the
   recipe already reproduced; scope to the headline claim, not every ablation).
   Pin the environment in `requirements.txt` / `environment.yml` to versions
   that work. Commit as you go.

8. **Run the full replication** via `scripts/run.py` (the CI entry point);
   capture metrics as JSON into `results/`. Commit.

9. **Write `FINDINGS.md`:** reproduced vs. reported numbers (table); what the
   recipe covered vs. what you filled; gaps (hyperparameters, preprocessing,
   omitted architecture details) and where/why it diverged. Commit and push.

10. **Publish and finish.** Confirm `.github/workflows/pages.yml` (site + PDF
    report) and `.github/workflows/package.yml` (ZIP) run green; set
    Settings → Pages → Source: GitHub Actions. Keep `SKILL.md` (and
    `replication_skill.md`, if you found one) truthful to what you actually did.
    **Stop / hand back** when `FINDINGS.md` reports at least one headline number
    with its reproduced value, `scripts/run.py` runs end-to-end from a clean
    clone (or documents the un-automatable data step), the repo is public and
    pushed, and the Pages deployment is green.

---

## Pointers

- Methodology / definition of done: `SKILL.md`.
- Long-horizon items: `todo.md`.
- Completed work + replication milestones (chronological): `devlog.md`.
- Narrative history: `git log`.
