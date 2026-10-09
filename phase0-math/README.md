# Phase 0 — Math Floor (surgical 20%)

The load-bearing math every world-models paper assumes. Install it ONCE, properly, with code attached so it registers. CPU only. No full stats course — just the specific 20% the later builds use.

**Method:** never learn a concept abstractly. For each concept, read the mapped section, then write a tiny script that makes it *visible* (sample it, plot it, watch it change). The script is the proof you understood it.

## Primary book — *Mathematics for Machine Learning*
Deisenroth, Faisal & Ong. **Free PDF: [mml-book.github.io](https://mml-book.github.io/)** (direct: [mml-book.pdf](https://mml-book.github.io/book/mml-book.pdf)).

Written for exactly this situation — a coder filling the math gap, every concept motivated toward ML rather than taught as pure math. **Do NOT read cover to cover.** Read only the sections mapped below; the rest of the book is Phase 1+ territory or beyond scope.

### Concept → chapter map (read in this order)
mml-book ch. 6 is the backbone. **Where it's strong, use it; where it's thin (KL), use the dedicated source in the next column — don't force the book to cover KL, it doesn't do it well.**

| Phase 0 concept | Where to read it |
|---|---|
| Vectors/matrices refresh (only if rusty) | mml-book **Ch. 2** (skim 2.1–2.4) |
| Gradients / chain rule refresh | mml-book **Ch. 5.1–5.2** (keep light — you BUILD this in Phase 1) |
| Probability, random variables, expectation | mml-book **Ch. 6.1–6.4** |
| The Gaussian (1-D and multivariate) | mml-book **Ch. 6.5 — Gaussian Distribution** |
| Maximum likelihood | mml-book **Ch. 6.3** (sum/product rules) + the MLE treatment in **Ch. 8.3** |
| **KL divergence** *(the gap — mml-book barely covers it)* | **not mml-book** → see the KL block below |

### KL divergence — the one you source elsewhere
KL is the heart of the VAE and Dreamer losses, so learn it properly from a source that actually derives it. In order of preference:
1. **Goodfellow, *Deep Learning*, §3.13 "Information Theory"** — defines entropy → cross-entropy → KL cleanly, and states the asymmetry. [Free](https://www.deeplearningbook.org/contents/prob.html).
2. **StatQuest "Entropy / KL divergence" videos** — if the book is too dense; best plain-language intuition.
3. **The Gaussian-vs-Gaussian KL derivation** — the *exact* closed form you'll reuse in the VAE. Lilian Weng's "From Autoencoder to Beta-VAE" post derives it; you'll meet it again in Phase 2, so even a first skim now pays off.

Then make it yours: write the script that plots KL(p‖q) rising as you slide two Gaussians apart, and show KL(p‖q) ≠ KL(q‖p). The plot IS your understanding.

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
