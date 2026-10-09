# World Models — Mastery Plan

**Goal (restated 2026-10-09):** genuine EXPERTISE in world models — understand the science and math underneath, not just build a working one. The marker of success: *you can read any new world-models paper and immediately place it* — "this is Dreamer with X swapped for Y, here's the tradeoff." That ability is the whole target.

**Profile:** strong distributed-systems engineer (ex-Amazon), fluent coder, light on ML/stats, good at math as a kid. Learns by self-driving + building.

**Budget:** 1 hr/day, 7 hrs/week. Daily cadence (not weekend bursts) — the daily habit is the engine. Realistic: **~7-9 months to read-any-paper depth.**

**Compute:** Mac CPU for Phases 0-3. GPU needed from Phase 4 Week 1 (Colab / cloud box / local card — arrange before Phase 4).

**Working mode:** self-drive. Jeremy on demand when stuck. Socratic deep-dives available for the hard concepts (ELBO, KL, RSSM) if you want them. **A milestone test at every phase boundary** — you don't advance until you pass it. The tests are your guardrail against fuzzy understanding.

---

## The method — the three-beat loop (do this EVERY topic)

This is why things will register deeply instead of evaporating. Repeat per topic:

1. **Theory beat (short, surgical):** learn the *specific* math the next build needs, and immediately make it concrete with a tiny script (sample a Gaussian, plot it, watch KL rise as two Gaussians separate). Never learn math with nothing to attach it to.
2. **Build beat (the bulk):** build the real thing from scratch. The theory is now load-bearing, so it sticks.
3. **Explain beat (compounding):** write the idea in your own words in `TERMS.md`. If you can't write it, you don't understand it yet — go back. This is retrieval practice, the highest-leverage habit here. You already do this; lean in hard.

Ratio shifts over time: Phase 0-2 ≈ 40% theory / 60% build; Phase 3-5 ≈ 20% theory / 80% build.

**The one rule:** build the core idea from scratch once; clone-and-modify the plumbing. Read in service of the build, never ahead of it. Every unknown symbol is your next 30-min study unit — targeted, not upfront.

---

## Daily unit rule (because 1 hr/day)

Every task below is sized to ONE sitting (~45-60 min) with a clear done-state, so you never sit down unsure what today's hour is for. If a build is bigger than a day, it's split into daily sub-tasks. End each day with one line in the Progress Log: what you did, what broke, what clicked.

---

## Phase 0 — Math floor (surgical 20%) · ~3 weeks · CPU
*The block your earlier plan skipped. For a mastery goal you install these properly ONCE — they recur in every paper.*
Folder: `phase0-math/`

- [ ] Random variables + expectation. Script: estimate E[X] for a die and a Gaussian by sampling; watch the sample mean converge.
- [ ] Gaussians (1-D then multivariate). Script: sample, plot, change μ and σ, see what each does.
- [ ] Sampling + the law of large numbers. Script: Monte-Carlo estimate of an integral.
- [ ] Maximum likelihood. Script: fit a Gaussian's μ,σ to data by maximizing likelihood; see it match the sample stats.
- [ ] KL divergence. Script: KL(p‖q) between two Gaussians; plot it rising as they separate; note it's asymmetric.
- [ ] Gradients / chain rule refresh (fast — you'll BUILD this in Phase 1). Just re-fluent on partial derivatives.

**🎯 Milestone test 0:** explain, in writing + a plot, what KL divergence measures and why it's asymmetric; derive the MLE for a Gaussian's mean. (Jeremy sets + checks.)

---

## Phase 1 — Mechanism from scratch · ~4 weeks · CPU
*Backprop and the Transformer stop being magic. Pure build. Non-negotiable for mastery.*
Folder: `phase1-nn-floor/`

- [ ] Karpathy "Zero to Hero" #1 — micrograd. **Type it yourself**, don't watch.
- [ ] Karpathy #2-4 — makemore (bigram → MLP). Build the MLP char model; watch loss drop.
- [ ] Karpathy "Let's build GPT" — nanoGPT on tinyshakespeare.

**🎯 Milestone test 1:** hand-derive the gradient of a 2-layer net on paper and show your micrograd matches; explain attention (Q/K/V) and why a Transformer beats an RNN on long sequences.

---

## Phase 2 — Latent variables (the conceptual heart) · ~3 weeks · CPU
Folder: `phase2-vae/`

- [ ] Theory beat: derive the **ELBO** on paper (you can now, after Phase 0). Read Lilian Weng "From Autoencoder to Beta-VAE".
- [ ] Build: a VAE on MNIST from scratch — encoder → latent → decoder, ELBO loss.
- [ ] Sample from the latent, generate digits; walk the latent space and watch digits morph.
- [ ] Explain beat: why the reconstruction term and KL term fight; what the **reparameterization trick** buys (and why you can't backprop through a sample without it).

**🎯 Milestone test 2:** one-sentence each on the two ELBO terms; explain reparameterization and why predicting in latent space beats pixels. ← *This is the ~1-month "can explain latents fluently" checkpoint.*

---

## Phase 3 — Sequences + just-enough RL · ~4 weeks · CPU
Folder: `phase3-rl/`

- [ ] RL vocabulary only (David Silver L1-2 OR Sutton & Barto ch.1,3). Stop at: state, action, reward, return G_t, discount, policy, value function. **Do not take a full RL course** — it's a supporting character.
- [ ] Build/clone: a working actor-critic or PPO on CartPole (clone CleanRL single-file, read it, modify the net/reward).
- [ ] Bridge build: extend your VAE with a tiny **MDN-RNN** that predicts the next latent from (latent, action) on a simple env.
- [ ] Roll it forward — "dream" a few steps, decode the imagined latents to frames.

**🎯 Milestone test 3:** define return G_t and policy-vs-value without notes; trace where actor loss and critic loss come from in the code; explain what your MDN-RNN's π,μ,σ outputs are.

---

## Phase 4 — Literature as primary sources + reimplementation · ~10 weeks · **GPU**
*Read each paper properly AND reimplement the core loop. Backfill-on-demand stays on.*
Folder: `phase4-papers/`

- [ ] **World Models (Ha & Schmidhuber, 2018).** Read properly. Build: connect VAE + MDN-RNN + small controller on CarRacing (or lighter env); train controller (CMA-ES / simple RL). Goal: an agent that learned partly *in its own dream*; explain V/M/C cold.
- [ ] **Dreamer / DreamerV3 (Hafner et al.). THE credential.** Read both. Build: a *small* Dreamer on a toy env — the RSSM (deterministic h + stochastic s), ELBO world-model loss with KL balancing, imagination actor-critic. Clone danijar/dreamerv3 for scaffolding; **write the RSSM loss + imagination rollout yourself.**
- [ ] **JEPA / V-JEPA (LeCun). The frontier.** Read. Build: minimal JEPA — predict the embedding of a masked/future part, EMA target encoder, no decoder. Remove the stop-gradient once on purpose and watch representation collapse.
- [ ] **Genie (DeepMind, 2024). Controllable simulators.** Read, focus on the latent action model (actions learned from unlabeled video). Design doc of a tiny version — no full reimplement.
- [ ] **One RECENT paper (last 12 months).** Pick one when you get here; reproduce its core. This keeps you *current*, not just historical. (Jeremy will pull a shortlist when you reach this.)

**🎯 Milestone test 4:** explain the RSSM prior-vs-posterior KL and why training happens in imagination; argue why predict-in-embedding (JEPA) beats predict-in-pixels; explain how Genie gets a controllable model from action-less video.

---

## Phase 5 — The math INSIDE the models (the expert layer) · ~4 weeks
*What turns "I built a Dreamer" into "I understand world models." Circle back and make rigorous what you used.*
Folder: `phase5-depth/`

- [ ] RSSM prior/posterior KL, free bits, KL balancing — derive, don't hand-wave.
- [ ] Why imagination training works (and when it fails — model exploitation).
- [ ] The information-theoretic case for latent/embedding prediction (JEPA) — what collapse is, formally, and what prevents it.
- [ ] Write a synthesis doc: "the three traditions world models fuse (latent-variable models, sequence modeling, control/RL) and how each major paper combines them."

**🎯 Milestone test 5 (the capstone):** pick a world-models paper Jeremy has NOT shown you. In writing: place it in the lineage, name what's new vs. prior work, and critique one design choice with a tradeoff. Pass = you're at read-any-paper depth. **This is the finish line of the plan** — after it, you follow the field yourself.

---

## Milestones at a glance (your "do I actually understand it" checkpoints)
- **End Phase 0 (~Wk 3):** probability + KL are not fuzzy.
- **End Phase 2 (~Wk 10):** can explain latents/VAEs/ELBO fluently.
- **End Phase 4 Dreamer (~Wk 24):** reimplemented a working small Dreamer. The credential.
- **End Phase 5 (~Wk 30):** can read, place, and critique any new world-models paper. Mastery.

---

## Progress log
_(one dated line per day: built / broke / clicked / useful link)_

- 2026-10-09 — Mastery plan created (supersedes the earlier build-heavy tracker). Starting Phase 0, task 1.
