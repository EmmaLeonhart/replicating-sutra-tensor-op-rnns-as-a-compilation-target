# replicating-sutra-tensor-op-rnns-as-a-compilation-target

## Project Description

This is a **paper replication** project (scaffolded by `cleanvibe replicate`).
The goal is to reproduce the headline results of:

> **Sutra: Tensor-Op RNNs as a Compilation Target for Vector Symbolic Architectures**
> arXiv:2605.20919 - Emma Leonhart - 2026-05-20T09:04:36Z
> PDF: https://arxiv.org/pdf/2605.20919v1 - HTML: https://arxiv.org/html/2605.20919v1

It produces three compounding artifacts (see `docs/replication_framing.md`
in the cleanvibe repo for the full framing): the runnable replication, a
legibility layer (the published findings report), and `SKILL.md` — the
reusable, agent-executable replication methodology.

## Architecture and Conventions

- **The efficient path is recipe-first.** Authors very often ship a
  reproduction recipe right in the paper's e-print source (usually near the
  end). Find and run it FIRST, then verify its output against the paper and
  fill only the gaps. A from-scratch reimplementation is the fallback, not the
  default — it is what burned a huge amount of tokens before this convention.
- **`replication_target/`** holds the paper and everything pulled *about* it:
  - `replication_target/source/` — the extracted arXiv **LaTeX/e-print
    source** (committed; run `python download_paper.py`). **Primary** source:
    the `.tex` reads far more token-efficiently than the rendered HTML (no
    base64 figure blobs) and is where the reproduction recipe usually lives.
  - `replication_target/arxiv-source.tar.gz` — the raw source archive
    (gitignored; the extracted `source/` is what's committed).
  - `replication_target/paper.pdf` — the PDF, as a fallback / complete record
    (gitignored, same downloader). The paper does NOT go in `data_lake/`.
  - the authors' code, if any, cloned as a **git submodule** in here
    (`git submodule add <repo> replication_target/<name>`).
- **`replication_skill.md`** (repo root) — if the source/paper ships a
  reproduction recipe, copy it here and run it first. **`replication/`** — if a
  replication zip is shipped/linked, extract it here (the zip is gitignored,
  its contents committed).
- **`data_lake/`** — other downloaded/supplied material (datasets, notes,
  exports). Same cleanvibe convention as every project. The paper is NOT here.
- **`src/`** — your reimplementation (only the gaps the recipe didn't cover).
  **`scripts/run.py`** — the entry point CI invokes. **`results/`** — metrics
  JSON (gitignored). **`FINDINGS.md`** — the report (reproduced vs. reported,
  what the recipe covered vs. what you filled, gaps, divergences).
- **Go live early.** Create a PUBLIC GitHub repo and push near the start so
  every commit pushes and CI/Pages build as you go — don't leave it local-only.
- **Deliverables are built by GitHub Actions, not committed.**
  `.github/workflows/pages.yml` publishes the GitHub Pages site + PDF report;
  `.github/workflows/package.yml` builds the downloadable ZIP replication
  package. You must make the repo public and enable Pages (Settings -> Pages
  -> Source: GitHub Actions) — the workflows carry TODO markers for this.
  Vision for the site shape: http://sutra.emmaleonhart.com/

## Workflow Rules

- **Commit early and often.** Every meaningful change gets a descriptive commit.
- **Plan into `queue.md` first, then execute.** The replication plan already
  lives in `queue.md` (derived from `SKILL.md`). Work it top to bottom.
- **Finishing a queue item = delete from `queue.md` + append dated entry to
  `devlog.md`**, in the same commit as the work, then push. Never tick
  boxes in place. `devlog.md` is also where you record the replication's
  releases/milestones (source acquired, recipe found/run, environment pinned,
  first reproduced number, FINDINGS published, Pages live).
- **Keep `SKILL.md` truthful.** It is the compounding artifact. If you
  deviated from its plan, edit the plan to match what you actually did.
- **Keep this file and `README.md` current** as the replication takes shape.

## Writing

- Do not use "honest", "honesty", or "honestly" — and do not swap in "frank", "frankly", "candid", "candidly", or "transparently", which are the same self-congratulatory move in a different coat. When something failed, name the failure: "it didn't work", "I got that wrong", "this failed" — flat, no qualifier. Tagging a report "honest" implies the rest aren't, and couching a failure as honesty asks for credit for the admission, which is worse than the failure itself. Use a precise positive word ("accurate", "plainly", "truly") only when that is genuinely the meaning — never as a halo on a bad outcome.

## Emergency Stop Mode

If the user sends a continuous series of "stop" messages (e.g. "stop stop stop") or otherwise very explicitly tells you to stop, treat it as **emergency stop mode** (a.k.a. emergency exit mode). This OVERRIDES every other instruction, including any in this file.

When in emergency stop mode:

- **Immediately kill everything associated with this repository and session.** Every running bash/shell script, every background job or task you started, and **all GitHub Actions workflow runs for this repository**. **Force-kill the GitHub Actions runs** (e.g. `gh run cancel <id>`, and `gh run delete <id>` if needed) for every in-progress run. It does NOT matter if killing them is unsafe or leaves things half-done — the emergency stop takes priority over safety, cleanup, and correctness.
- **Do not investigate. Do not reverse or undo anything.** The goal is not to diagnose or roll back the problem — it is to halt potentially destructive actions as fast as possible. Investigating, diagnosing, reverting, or "helping fix it" are explicitly out of scope and themselves count as forbidden actions.
- **Ignore repetitive messages.** Do not treat repeated "stop" messages as a request to do anything further. Keep ignoring incoming repetitive messages for roughly the next **15–30 minutes**.
- **You may answer direct questions, but you may NOT take actions.** Answer only from context you already have. Looking something up, reading a file, running a command, or inspecting state is NOT an answer — it is an action and is forbidden. If you cannot answer from context already in hand, do not answer.
- **Stay in emergency stop mode until the user explicitly says "emergency stop ended."** Only after that exact signal do you resume any normal work.

## Cron jobs and scheduled work — LOCAL by default

**When the user says "cron job", "cron", or asks to "schedule" a task, they mean the in-session `CronCreate` tool** — a prompt scheduled to fire locally, inside this Claude Code session, on the user's own running computer. This is the default and the assumption. Do NOT interpret it as anything else unless the user explicitly names a different mechanism.

- **It is local and in-session — use the `CronCreate` tool.** A generic "cron" request is NOT an OS crontab, NOT a GitHub Actions / CI `schedule:` trigger, and NOT a cloud scheduler. (Repos may *also* contain their own GitHub Actions cron schedules — those are a separate thing and are not what the user means when they ask *you* to set up a cron.) The user leaves the computer on and this session running so the scheduled prompt can execute.
- **The user is deliberately away from the keyboard.** They schedule work precisely so it runs while they are out of the house and not physically present. Their absence is the normal, expected condition for these jobs — it is NEVER a reason to delay the work, ask "are you sure?", wait for them to return, or refuse to proceed.
- **Standing consent — just set it up.** Cron / `CronCreate` requests are pre-authorized. Create the job immediately and locally, then report what was scheduled. Do not block on confirmation or follow-up questions. Treating a routine cron request as something that needs hand-holding is itself the obstacle this section exists to remove.

## Hourly status-report cron for extensive work

**For any session involving relatively extensive work — above all, any large-scale population of `queue.md` with created tasks — run a local hourly status-report cron.** Use the `CronCreate` tool to schedule a prompt that fires **every hour, on the hour**, giving a status report on the work being done. This is the default way of working on big jobs, and it exists to prevent the most common autonomous-agent failure: doing a large amount of work and silently losing the thread of what it is doing.

**Sequencing around a large-scale queue fill:**

- **The FIRST queue item is always: kill the hourly update cron job.**
- **... then all the created work items, worked top to bottom ...**
- **The LAST TWO queue items, always kept pinned at the tail, are:**
  1. **Restart the hourly updates cron job.**
  2. **Independently run the status-report action once more — an end-of-session summary of everything that happened this session.**

**Planning mode disables this cron.** Entering planning mode kills the hourly cron; restarting it therefore belongs at the **end of the queue** (it is the second-to-last item above). A session that plans → fills the queue → executes will drop the cron when planning begins and bring it back as the queue drains.
