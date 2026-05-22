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

## Done — "Sutra" (arXiv:2605.20919) replication complete

The recipe-first replication is finished and handed back. All headline claims
reproduced (most exactly); deliverables live and green. See `FINDINGS.md` for
the reproduced-vs-reported tables and `devlog.md` for the milestone trail.

Nothing queued. Possible future polish (not blocking, only if asked):
- Have `scripts/run.py` support a `--reuse` mode that assembles
  `results/metrics.json` from existing `*_results.json` without re-running the
  ~15 min capacity/crosstalk sweeps.
- Optionally pin a `requirements.txt` (torch, numpy, transformers, torchhd) for
  a fully scripted local setup.

---

## Pointers

- Methodology / definition of done: `SKILL.md`.
- Long-horizon items: `todo.md`.
- Completed work + replication milestones (chronological): `devlog.md`.
- Narrative history: `git log`.
