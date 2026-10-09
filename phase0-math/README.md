# Phase 0 — Math Floor (surgical 20%)

The load-bearing math every world-models paper assumes. Install it ONCE, properly, with code attached so it registers. CPU only. No calculus/stats course — just the specific 20% the later builds use.

**Method:** never learn a concept abstractly. For each concept, read a little, then write a tiny script that makes it *visible* (sample it, plot it, watch it change). The script is the proof you understood it.

## Concepts to own (the whole phase)
- **Random variables & expectation** — what E[X] is; why "sample and average" estimates it.
- **The Gaussian** — 1-D (what μ and σ each control) and multivariate (diagonal covariance; why a latent `z` is "a point in a Gaussian").
- **Sampling & the law of large numbers** — why averaging many samples approximates an expectation (Monte Carlo).
- **Maximum likelihood (MLE)** — "likelihood of data under a model"; why the MLE of a Gaussian's mean is just the sample mean.
- **KL divergence** — what KL(p‖q) measures ("extra surprise from using q when truth is p"); why it's asymmetric. *This one is non-negotiable — it's the heart of the VAE and Dreamer losses later.*
- **Gradients / chain rule** — just a refresh. You'll BUILD backprop in Phase 1, so don't over-invest here.

## Build (make each concept concrete)
A handful of ≤30-line scripts, one per concept — e.g. die-roll running-mean convergence, Gaussian pdf plots, a 2-D Gaussian scatter you stretch via covariance, a Monte-Carlo estimate of π, a KL-vs-distance plot. Keep them in this folder.

## References (pull as needed, don't read cover to cover)
- **3Blue1Brown** — "Essence of Linear Algebra" + the probability videos. Best intuition-first source for exactly this 20%.
- **StatQuest (Josh Starmer)** — short YouTube explainers for expectation, Gaussians, likelihood, KL. Watch only the ones you need.
- **Deep Learning (Goodfellow, Bengio, Courville), ch. 3 "Probability and Information Theory"** — the reference for this phase. Read §3.1–3.13; it covers RVs, expectation, Gaussians, MLE, and KL in ~15 pages. [Free online](https://www.deeplearningbook.org/).
- `scipy.stats`, `numpy.random`, `matplotlib` — your lab bench.

## 🎯 Milestone test 0 (ping Jeremy when ready)
In writing + plots:
1. What KL divergence measures, and why KL(p‖q) ≠ KL(q‖p) — with your plot as evidence.
2. Argue that the MLE for a Gaussian's mean is the sample mean.
3. One sentence each: expectation, sampling, maximum likelihood.

Pass this and Phase 1 (pure building) begins.

## Log
_(a line whenever something clicks or breaks)_
