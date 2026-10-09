# World Models — learning from scratch

A build-first journey to genuine expertise in **world models** — the science and math underneath, not just a working implementation. The goal: reach the point where I can read any new world-models paper and place it precisely (lineage, what's new, the tradeoffs).

Learning in public, 1 hour a day.

## How this is structured
- **[`MASTERY-PLAN.md`](MASTERY-PLAN.md)** — the full 5-phase plan, milestone tests, and the method (the "three-beat loop").
- **[`TERMS.md`](TERMS.md)** — my running glossary. Every term I had to look up, in my own words. This file *is* the expertise accumulating.
- **[`WEDGE.md`](WEDGE.md)** — a friction log: anything expensive/broken/missing I hit while building. Startup ideas come bottom-up from here.
- **`phase0-math/`** → **`phase5-depth/`** — the actual work, phase by phase.

## The path
0. **Math floor** — the load-bearing 20% (probability, Gaussians, KL, MLE).
1. **Mechanism from scratch** — backprop, Transformers (Karpathy Zero-to-Hero).
2. **Latent variables** — VAE on MNIST, the ELBO.
3. **Sequences + just-enough RL** — actor-critic, a mini MDN-RNN that "dreams".
4. **The literature** — World Models → Dreamer → JEPA → Genie, read as primary sources and reimplemented.
5. **The math inside the models** — RSSM KL, imagination training, why predict-in-embedding wins.

## The one rule
> Build the core idea from scratch once; clone-and-modify the plumbing. Read in service of the build, never ahead of it.

## Setup
```bash
python3 -m venv .venv && source .venv/bin/activate
pip install numpy matplotlib scipy   # per-phase deps added just-in-time
```
