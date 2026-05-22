# Claims and reported numbers

The recipe (`replication/sutra-replication-package/SKILL.md`) covers **every**
headline claim, so this file is the reported-number reference the findings
table is checked against — not a from-scratch reimplementation spec.

## Headline claim
One artifact is simultaneously a logic program and a trainable neural network:
a typed functional language (`.su`) whose compiled forward pass is a PyTorch
tensor-op graph over a frozen embedding substrate. Validated two ways:
(1) the same program decodes bundles at 100% through width k=8 on four frozen
substrates where textbook Hadamard binding has already collapsed; (2) PyTorch
autograd flows through the actually-compiled graph to train a fuzzy-rule
classifier from chance to 100%.

## §3.1 — Rotation vs. Hadamard capacity (reported, paper.tex.body L386–389)
Decode accuracy, 10 trials/k, 84-word codebook, decode = unbind + argmax-cosine.

| substrate (dim)         | rot k=8 | rot k=48 | Had k=8 | Had k=48 |
|-------------------------|---------|----------|---------|----------|
| nomic-embed-text (768)  | 100.0%  | 93.3%    | 87.5%   | 48.3%    |
| all-minilm (384)        | 100.0%  | 42.3%    | 7.5%    | 1.7%     |
| mxbai-embed-large (1024)| 100.0%  | 72.1%    | 2.5%    | 1.0%     |
| ESM-2 (320)             | 100.0%  | 44.2%    | 28.7%   | 4.2%     |

- Rotation ≥ 95% at k=8 on every substrate; Hadamard collapses by k=8 on the
  two harder text substrates. SKILL asserts `rotation k=8 >= 0.95`.
- Rotation reversibility round-trip: mean ‖unbind(R,bind(R,x))−x‖ = 1.5×10⁻¹⁵.
- ESM-2 = `facebook/esm2_t6_8M_UR50D` (protein LM, no NL exposure) — the
  substrate-agnostic check. SKILL asserts ESM-2 rot k=8 ≥ 0.95, Had k=48 ≤ 0.10.

## §3.1.1 — Chained bind/unbind crosstalk (reported, L405–408)
Chain lengths L, 20 trials, bundle width 4. Raw accuracy 100% through L=2 on
every substrate; falls to chance (1/84 ≈ 1.2%) by L=8. SKILL asserts
chain=1 == 1.0 and chain=8 ≤ 0.05. Scopes §3.1 to single-cycle records.

## §3.4 — First-class loops as soft-halt RNN cells
Runtime data-dependent loops compile to self-halting RNN cells; T (tick budget)
is a runtime budget, work bounded by the soft-halt cell. Reproduced via the
23-test `test_loop_function_decl.py` and the T∈{50,200,10000} invariance check
(`do_while_adder.su` → 11 across all T).

## §3.5 — Embedded codebook (SutraDB FFI)
7 tests in `test_sutradb_embedded.py` (skip unless the Rust `sutra-ffi` is
built).

## §3.6 — Differentiable training through the compiled graph (reported, L682–693)
K=5, 50 words (5 classes × 10), nomic-embed-text 768-d frozen, 5 learnable
prototypes random-init, rule_i = AND(sim(x,p_i), AND_{j≠i} NOT(sim(x,p_j)))
composed from the compiler's emitted Lagrange–Kleene polynomials. Full-batch
cross-entropy, Adam lr=0.01, 30 epochs, 3 seeds (0–2).
- **Before 18.7 ± 9.5%** (chance = 20%) → **after 100.0 ± 0.0%**.
- Gradients flow through the emitted graph every seed
  (`grads_through_emitted_graph=True`). Batched ≈230 s on CPU; per-sample
  Python driver gives bit-identical result in ≈6.2 h. In-sample (verifies
  backprop reaches every prototype through emitted ops; not generalization).

## §3.7 — Trained scalar gain baked back into `.su` (reported, L727–763)
rule_i = AND(w·sim(x,p_i), AND_{j≠i} NOT(w·sim(x,p_j))). K=3, 24 words
(3 classes × 8), nomic-embed-text frozen, Adam lr=0.02, 30 epochs, 2 seeds.
- **Before 33.3 ± 5.9%** (chance = 33.3%) → **after 100.0 ± 0.0%**.
- Learned gain **w\* = 1.434 ± 0.004** (moves from init 1.0; not a tautology).
- w\* baked into fresh `.su` as a literal, recompiled → logits within
  **≈2×10⁻⁷** of the parametric model (`round_trip_ok(all)=True`).

## §4 / §5 — Compiler pipeline + demo corpus
Paper says "245+ tests, full suite green" and "27 .su files / 10-program smoke
test". SKILL block expects `237 passed, 7 skipped`.

## Compute envelope (decides CI-runnability)
- Compiler pytest suite: ~14 s (paper) — but needs torch; runs offline.
- Capacity sweeps + crosstalk + differentiable training: need **local Ollama**
  with three embedding models (network/daemon dependency GitHub Actions can't
  easily provide) and the §3.6 batched run is ≈230 s CPU. ⇒ heavy experiments
  are **not CI-runnable**; CI runs the offline subset, full set runs locally.

## References — checked (queue item 5)
All 25 references are real, standard works and the load-bearing ones say what
the paper claims:
- Siegelmann & Sontag 1992 — rational-weight RNNs Turing-complete ⇒ grounds
  "tail-recursive loops as RNN cells".
- Ethayarajh 2019 / Gao 2019 — LLM-embedding anisotropy / representation
  degeneration ⇒ the stated motivation for §3.7's learned scalar gain.
- Plate 1995, Kanerva 2009, Gayler 2003 — VSA binding/bundling foundations;
  "Hadamard = textbook MAP-VSA" is accurate.
- Lin et al. (ESM-2, Science 2023), Kingma & Ba (Adam, 2015), Heddes (TorchHD),
  Li (Scallop), Manhaeve (DeepProbLog) — all real, used as stated.
