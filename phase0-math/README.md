# Phase 0 — Math Floor (surgical 20%)

The load-bearing math every world-models paper assumes. You install it ONCE, properly, with code attached so it registers. ~3 weeks at 1 hr/day. CPU only.

**Rule for this phase:** never learn a concept abstractly. Each day = read ~15 min, then write a ≤30-line script that makes the concept *visible* (plot it, sample it, watch it change). The script is the point.

## Setup (day 1, 10 min)
```bash
cd ~/Documents/world-models
python3 -m venv .venv && source .venv/bin/activate
pip install numpy matplotlib scipy
```

## Daily tasks (one per sitting)

### Week 1 — distributions & expectation
- **Day 1** — Random variables + expectation. Script: simulate 10k die rolls, plot the running sample mean, watch it converge to 3.5. Then do the same for `numpy`'s Gaussian. *Done when:* you can say what E[X] means in one sentence.
- **Day 2** — The Gaussian, 1-D. Script: plot `scipy.stats.norm` pdf for three (μ,σ) pairs on one chart. *Done when:* you can say what μ and σ each control, cold.
- **Day 3** — Multivariate Gaussian. Script: sample 2-D points from a Gaussian with a diagonal covariance, scatter-plot them; change the covariance, see the cloud stretch. *Done when:* you see why a latent `z` being "a point in a Gaussian" makes sense.
- **Day 4** — Sampling + law of large numbers. Script: Monte-Carlo estimate of π (points in a circle) or ∫. *Done when:* you trust that "sample and average" approximates an expectation.
- **Day 5** — Catch-up / re-explain day. Write the week's terms into `../TERMS.md` in your own words. (The explain beat.)

### Week 2 — likelihood & KL
- **Day 6** — Maximum likelihood, concept. Read what "likelihood of data under a model" means.
- **Day 7** — MLE in code. Script: generate data from a known Gaussian, recover μ,σ by maximizing likelihood (or just compute sample mean/var and show they ARE the MLE). *Done when:* you can state why MLE for a Gaussian's mean is just the average.
- **Day 8** — KL divergence, concept. Read: KL(p‖q) = "extra surprise from using q when truth is p." Note it's asymmetric.
- **Day 9** — KL in code. Script: compute KL between two 1-D Gaussians analytically; plot KL as you slide q's mean away from p's. *Done when:* you can explain the asymmetry with your plot.
- **Day 10** — Re-explain day. KL + MLE into `../TERMS.md`, your words.

### Week 3 — gradients & consolidation
- **Day 11** — Chain rule / partial derivatives refresh. You'll BUILD backprop in Phase 1, so just get fluent on paper. Script optional: `numpy` numerical gradient of a 2-var function.
- **Day 12** — Why gradients matter for learning: one paragraph connecting "gradient = direction that reduces loss."
- **Day 13-14** — Buffer / milestone-test prep.

## 🎯 Milestone test 0 (end of phase — ping Jeremy)
Produce, in writing + plots:
1. What KL divergence measures, and why KL(p‖q) ≠ KL(q‖p), with your plot as evidence.
2. A derivation (or clear argument) that the MLE for a Gaussian's mean is the sample mean.
3. In one sentence each: expectation, sampling, maximum likelihood.

Pass this and Phase 1 (building) is where the fun starts.

## Log
_(dated line per day)_
