# Phase 0 — Math Floor (surgical 20%)

The load-bearing math every world-models paper assumes. Install it ONCE, properly, with code attached so it registers. CPU only. No full stats course — just the specific 20% the later builds use.

**Method:** never learn a concept abstractly. For each concept, read the mapped section, then write a tiny script that makes it *visible* (sample it, plot it, watch it change). The script is the proof you understood it.

## Primary book — *Mathematics for Machine Learning*
Deisenroth, Faisal & Ong. **Free PDF: [mml-book.github.io](https://mml-book.github.io/)** (direct: [mml-book.pdf](https://mml-book.github.io/book/mml-book.pdf)).

Written for exactly this situation — a coder filling the math gap, every concept motivated toward ML rather than taught as pure math. **Do NOT read cover to cover.** Read only the sections mapped below; the rest of the book is Phase 1+ territory or beyond scope.

### Concept → chapter map (read in this order)
| Phase 0 concept | mml-book section |
|---|---|
| Vectors/matrices refresh (only if rusty) | **Ch. 2 — Linear Algebra** (skim 2.1–2.4) |
| Gradients / chain rule refresh | **Ch. 5 — Vector Calculus** (5.1–5.2; you'll BUILD this in Phase 1, so keep it light) |
| Probability, random variables, expectation | **Ch. 6 — Probability and Distribution** (6.1–6.4) |
| The Gaussian (1-D and multivariate) | **Ch. 6.5 — Gaussian Distribution** |
| Maximum likelihood | **Ch. 6.3 + 8.3** (MLE as a learning principle) |
| KL divergence *(non-negotiable — heart of the VAE & Dreamer losses)* | **Ch. 6.4.3** + info-theory notes; backfill with the companions below if the book is thin here |

> KL divergence is the one place mml-book is a little light. If 6.4.3 doesn't make it click, use *Deep Learning* ch. 3.13 (Goodfellow, [free](https://www.deeplearningbook.org/)) or the StatQuest KL video — then confirm it with your own KL-vs-distance plot.

## Companions (intuition + the thin spots)
- **3Blue1Brown** — "Essence of Linear Algebra" + probability videos. Watch alongside; makes the book's symbols move.
- **StatQuest (Josh Starmer)** — short explainers for expectation, Gaussians, likelihood, KL. Use for any concept that stays abstract.
- **Deep Learning (Goodfellow), ch. 3** — the fallback reference, especially for KL / information theory.
- `scipy.stats`, `numpy.random`, `matplotlib` — your lab bench. (mml-book also ships [Jupyter tutorials](https://mml-book.github.io/) for linear regression / PCA / GMM if you want worked code.)

## Build (make each concept concrete)
A handful of ≤30-line scripts, one per concept — die-roll running-mean convergence, Gaussian pdf plots, a 2-D Gaussian scatter you stretch via covariance, a Monte-Carlo estimate of π, a KL-vs-distance plot. Keep them in this folder.

## 🎯 Milestone test 0 (ping Jeremy when ready)
In writing + plots:
1. What KL divergence measures, and why KL(p‖q) ≠ KL(q‖p) — with your plot as evidence.
2. Argue that the MLE for a Gaussian's mean is the sample mean.
3. One sentence each: expectation, sampling, maximum likelihood.

Pass this and Phase 1 (pure building) begins.

## Log
_(a line whenever something clicks or breaks)_
