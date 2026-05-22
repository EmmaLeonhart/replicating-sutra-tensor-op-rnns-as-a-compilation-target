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

## Active — Replicate "Sutra" (arXiv:2605.20919)

Recipe-first path is in flight: source acquired, recipe found + run, public
repo live, Pages deployed, §3.1 capacity reproduced exactly. Remaining items
below; delete each in the same commit that completes it (and append to
`devlog.md`).

1. **Finish the §3.6/§3.7 differentiable-training runs and verify.** §3.6
   (`differentiable_training_compiled.py --k 5 ... --batched`) → confirm
   `grads_through_emitted_graph=True` and before≈18.7% → after 100.0%. §3.7
   (`differentiable_training_weighted.py --k 3 ...`) → confirm
   `round_trip_ok(all)=True` and w*≈1.43. Record reproduced numbers.

2. **Re-run crosstalk cleanly for all three substrates.**
   `experiments/crosstalk_chain.py` (the first run was killed to free CPU for
   §3.6 before its JSON was written). Confirm chain=1 100% / chain=8 chance on
   each substrate; capture the JSON.

3. **Fill the §3.6/§3.7 + crosstalk rows in `FINDINGS.md`** and push — the push
   redeploys the Pages site with the completed report (it currently serves the
   README because FINDINGS.md isn't pushed yet).

4. **Final tidy + confirm deliverables.** Confirm `package.yml` builds the ZIP
   (workflow_dispatch), `pages.yml` is green serving FINDINGS, `replicate.yml`
   stays green. Keep `SKILL.md` truthful. **Stop / hand back** when FINDINGS
   reports the headline numbers with reproduced values, `scripts/run.py` runs
   (documenting the local-only Ollama step), the repo is public + pushed, and
   Pages is green.

---

## Pointers

- Methodology / definition of done: `SKILL.md`.
- Long-horizon items: `todo.md`.
- Completed work + replication milestones (chronological): `devlog.md`.
- Narrative history: `git log`.
