# World Models — Mastery Plan

**Goal (restated 2026-10-09):** genuine EXPERTISE in world models — understand the science and math underneath, not just build a working one. The marker of success: *you can read any new world-models paper and immediately place it* — "this is Dreamer with X swapped for Y, here's the tradeoff." That ability is the whole target.

**Profile:** strong distributed-systems engineer (ex-Amazon), fluent coder, light on ML/stats, good at math as a kid. Self-driver — works backward from what's interesting.

**Pace:** ~1 hr/day. The plan is organized as **phases, not days** — do each phase in whatever daily rhythm fits, advance when the milestone test passes. Rough order-of-magnitude: ~7-9 months to read-any-paper depth, but the milestones are the real checkpoints, not the calendar.

**Compute:** Mac CPU is enough through Phase 3. GPU needed from Phase 4 (Colab / cloud box / local card — arrange before you start Phase 4).

**Working mode:** self-drive, Jeremy on demand when stuck. Socratic deep-dives available for the hard concepts (ELBO, KL, RSSM) on request. **A milestone test gates every phase** — don't advance until you pass it. The tests are the guardrail against fuzzy understanding, which is the one failure mode that kills a mastery goal.

---

## The method — the three-beat loop (apply to every concept)

Why things register deeply instead of evaporating:

1. **Theory beat (surgical):** learn the *specific* idea the next build needs, and make it concrete with a tiny script (sample a Gaussian, plot it, watch KL rise as two Gaussians separate). Never learn math with nothing to attach it to.
2. **Build beat (the bulk):** build the core thing from scratch. Now the theory is load-bearing, so it sticks.
3. **Explain beat (compounding):** write the idea in your own words in `TERMS.md`. If you can't write it, you don't understand it yet. This is retrieval practice — the highest-leverage habit here.

Theory/build ratio shifts from ~40/60 early to ~20/80 late.

**The one rule:** build the core idea from scratch once; clone-and-modify the plumbing. Read in service of the build, never ahead of it. Every unknown symbol is your next study unit — targeted, not upfront.

---

## Phase 0 — Math floor (the surgical 20%) · CPU · `phase0-math/`
**Purpose:** install the specific math every paper assumes, once, properly. (The one block a build-first plan tends to skip — fatal for a mastery goal.)

**Concepts to own:** random variables & expectation · the Gaussian (1-D μ/σ, and multivariate / diagonal covariance) · sampling & law of large numbers (Monte Carlo) · maximum likelihood · **KL divergence** (and why it's asymmetric) · a chain-rule refresh.

**References:**
- *Deep Learning* (Goodfellow/Bengio/Courville), **ch. 3 "Probability and Information Theory"** — the anchor read, ~15 pages, covers all of the above. [Free online](https://www.deeplearningbook.org/).
- **3Blue1Brown** — "Essence of Linear Algebra" + probability videos (intuition).
- **StatQuest** — short explainers for expectation, Gaussians, likelihood, KL.

**Build:** a few ≤30-line scripts that make each concept visible (die-roll mean convergence, Gaussian pdf plots, 2-D Gaussian scatter, Monte-Carlo π, KL-vs-distance).

**🎯 Milestone 0:** in writing + plots — what KL measures and why it's asymmetric; argue the MLE of a Gaussian's mean is the sample mean; one sentence each on expectation/sampling/MLE.

---

## Phase 1 — Mechanism from scratch · CPU · `phase1-nn-floor/`
**Purpose:** make backprop and the Transformer stop being magic. Pure build. Non-negotiable.

**Concepts to own:** autograd / backpropagation · a full training loop (forward, loss, backward, optimizer step) · why batching + learning rate matter · attention (Q/K/V) and why Transformers beat RNNs on long sequences.

**References:**
- **Karpathy, "Neural Networks: Zero to Hero"** (free, YouTube) — micrograd → makemore → "Let's build GPT". This IS the phase.
- *Deep Learning* ch. 6 (feedforward nets) as a reference when a term bites.

**Build:** type out micrograd yourself; build the makemore MLP char model; build nanoGPT on tinyshakespeare.

**🎯 Milestone 1:** hand-derive the gradient of a 2-layer net and show your micrograd matches; explain attention and the RNN-vs-Transformer tradeoff.

---

## Phase 2 — Latent variables (the conceptual heart) · CPU · `phase2-vae/`
**Purpose:** own the idea at the center of every world model — compress to a latent, work in latent space.

**Concepts to own:** autoencoder vs variational autoencoder · the **ELBO** (reconstruction term vs KL term, and why they pull against each other) · the **reparameterization trick** (why you can't backprop through a raw sample) · why we predict in latent space, not pixels.

**References:**
- Kingma & Welling, **"Auto-Encoding Variational Bayes" (2013)** — the VAE paper. Read it after the blog below.
- **Lilian Weng, "From Autoencoder to Beta-VAE"** — the best single explainer.
- *Deep Learning* ch. 20.10.3 (VAEs) as backup.

**Build:** a VAE on MNIST from scratch (encoder → latent → decoder, ELBO loss); sample digits; walk the latent space and watch them morph.

**🎯 Milestone 2 (~1-month checkpoint):** one sentence each on the two ELBO terms; explain reparameterization and why latent-space prediction beats pixels.

---

## Phase 3 — Sequences + just-enough RL · CPU · `phase3-rl/`
**Purpose:** enough RL to understand what a controller/policy does; then bridge to dynamics prediction.

**Concepts to own:** state, action, reward, **return G_t**, discount, policy, value function · actor-critic / policy gradient (where actor loss and critic loss come from) · an MDN-RNN predicting the *next latent* from (latent, action). **Do not take a full RL course — it's a supporting character.**

**References:**
- **David Silver RL lectures 1-2** OR **Sutton & Barto, *Reinforcement Learning*, ch. 1 & 3** (free [online](http://incompleteideas.net/book/the-book.html)). Stop at the vocabulary.
- **CleanRL** — single-file reference implementations (clone, read, modify).

**Build:** run a working actor-critic/PPO on CartPole (via CleanRL); then extend your Phase-2 VAE with a tiny MDN-RNN that predicts the next latent and can "dream" a few steps forward.

**🎯 Milestone 3:** define G_t and policy-vs-value cold; trace actor/critic loss in code; explain your MDN-RNN's π/μ/σ outputs.

---

## Phase 4 — The literature + reimplementation · **GPU** · `phase4-papers/`
**Purpose:** read the field's primary sources AND reimplement the core loops. Backfill-on-demand stays on.

**Papers (the canon, in order):**
- **Ha & Schmidhuber, "World Models" (2018)** — [worldmodels.github.io](https://worldmodels.github.io/). *Build:* VAE + MDN-RNN + small controller on CarRacing; train the controller (CMA-ES / simple RL); get an agent that learned partly in its own dream. Explain V/M/C cold.
- **Hafner et al., Dreamer → DreamerV2 → DreamerV3 — THE credential.** *Build:* a small Dreamer on a toy env — the **RSSM** (deterministic h + stochastic s), ELBO world-model loss with KL balancing, imagination actor-critic. Scaffold from `danijar/dreamerv3`; **write the RSSM loss + imagination rollout yourself.**
- **LeCun et al., I-JEPA / V-JEPA — the frontier.** *Build:* minimal JEPA — predict the embedding of a masked/future part, EMA target encoder, no decoder. Remove the stop-gradient once on purpose and watch representation collapse.
- **DeepMind, "Genie" (2024) — controllable simulators.** Focus on the latent action model (actions learned from unlabeled video). *Write a design doc* of a tiny version — no full reimplement.
- **One RECENT paper (last 12 months)** — pick one here and reproduce its core, to stay current. Jeremy will pull a shortlist when you reach this.

**🎯 Milestone 4:** explain the RSSM prior-vs-posterior KL and why training happens in imagination; argue predict-in-embedding (JEPA) vs predict-in-pixels; explain how Genie learns control from action-less video.

---

## Phase 5 — The math INSIDE the models (the expert layer) · `phase5-depth/`
**Purpose:** turn "I built a Dreamer" into "I understand world models." Circle back and make rigorous what you used by rote in Phase 4.

**Concepts to own:** RSSM prior/posterior KL, free bits, KL balancing (derive, don't hand-wave) · why imagination training works and when it fails (model exploitation) · the information-theoretic case for embedding prediction (what representation collapse *is*, formally, and what prevents it).

**References:** the Dreamer and JEPA papers re-read closely; *Deep Learning* ch. 3 (information theory) revisited; Hafner's blog posts and any "RSSM explained" writeups you find useful.

**Deliverable:** a synthesis doc — "the three traditions world models fuse (latent-variable models, sequence modeling, control/RL) and how each major paper combines them."

**🎯 Milestone 5 (capstone):** pick a world-models paper Jeremy has NOT shown you; in writing, place it in the lineage, name what's new, and critique one design choice with a tradeoff. Pass = read-any-paper depth. **This is the finish line** — after it, you follow the field yourself.

---

## Milestones at a glance (the real checkpoints — advance on these, not dates)
- **Phase 0:** probability + KL are not fuzzy.
- **Phase 2:** can explain latents / VAEs / ELBO fluently.
- **Phase 4 (Dreamer):** reimplemented a working small Dreamer — the credential.
- **Phase 5:** can read, place, and critique any new world-models paper — mastery.

---

## Progress log
_(a dated line whenever something clicks, breaks, or ships — not per day, just when there's something to record)_

- 2026-10-09 — Mastery plan created (phase-level, supersedes the earlier build-heavy tracker). Repo set up. Phase 0 next.
