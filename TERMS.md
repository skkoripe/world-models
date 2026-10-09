# TERMS — my running glossary & study targets

**How to use:** every time you hit a word/symbol you can't define cold, add it here with a one-line "?" placeholder. Look it up ONLY when the current build forces you to — then replace the "?" with your own plain-English definition (not a copy-paste). This file is your expertise, visible and growing.

---

## Week 1 deliverable — write these two things

### Why I care / the startup itch (one honest paragraph)
> _(fill in after playing with worldmodels.github.io — be specific about what gripped you)_

### The core loop, in my own words
> see a frame → compress it to a latent (VAE) → RNN predicts a distribution over the next latent using (latent, action, memory) → sample one next latent → controller reads (latent, memory) and picks an action → action changes the environment → repeat.

---

## Still open — terms I've hit but can't yet define
_(look these up only when a build forces it; they're Phase 2+ study units)_

- **KL divergence** — ? (comes back in Phase 2, VAE)
- **ELBO** — ? (Phase 2, VAE — the VAE's training objective)
- **rollout** — ? (roughly: letting the model run forward several steps to generate a trajectory — nail it precisely in Phase 3/4)
- **backpropagation** — partially: measure how wrong the output is (loss), push that error backwards through the net, nudge every parameter to reduce it. Will make it exact by BUILDING it in Phase 1 (Karpathy micrograd).

---

## Defined ✅ (things I can now explain cold)

### The three environments (the most clarifying distinction)
- **Real environment** = reality (real roads, real physics).
- **RL environment (simulator)** = a *hand-coded* program that mimics reality (the CarRacing game engine). Humans wrote it.
- **World model** = a *learned* imitation of the environment — a neural net that taught itself the dynamics by watching. The model built it, not a human.

### World model
A function: **`world_model(current_state, action) → prediction of the next state`**. That's the whole essence. Everything else (VAE, RNN, latents) is just *how you implement that function well* when the state is something big like an image.

### latent (latent vector `z`)
The model's own compact, learned "code" for an observation — e.g. a 64×64×3 image (12,288 numbers) squeezed into ~32 numbers. Not pixels; an abstract summary ("car here, road curves left"). We predict in this space because it's cheap and keeps only what matters. "Latent" = hidden/internal.

### VAE (Variational Autoencoder)
The component that does the compression: **image → latent (encoder), and latent → image (decoder)**. It squeezes *space* (pixels). Trained to compress-then-reconstruct so the latent captures the essentials. (KL/ELBO details deferred to Phase 2.)

### RNN + hidden state (the "M" / memory part)
- The **RNN** predicts the **next latent** (NOT the next image — it never touches pixels). It handles *time*.
- The **hidden state `h`** is the RNN's **running memory** — a vector that summarizes everything relevant that happened so far ("its notes to itself"). Not a clock/timestamp — a *compressed summary of the past*. The net *learns* what to remember because remembering the right things lowers its prediction error.
- Update each step: `h' = RNN_update(h, z, a)`.

### The RNN's inputs — `(zₜ, aₜ, hₜ)`
- **`z`** = current latent (what the world looks like now).
- **`a`** = **action** (what the agent chose to do — `aₜ` = action at time step t). Needed because the next state depends on the action.
- **`h`** = hidden state (summary of the past).
- Subscript **`t`** just means "at time step t"; t, t+1, t+2 … are consecutive moments.

### stochastic / distribution output / MDN / mixture of Gaussians
- **stochastic** = uncertain/random: outputs a *distribution* of possible next states, not one fixed answer.
- The RNN does **not** output the next latent directly. It outputs a **vector of numbers that are the *parameters* of a distribution**, then a separate (non-learned) **sampling** step draws one concrete latent from it.
- For a **mixture of Gaussians**, those parameters are three lists: `π` (mixing weights, e.g. [0.6, 0.3, 0.1]), `μ` (the center of each blob — each μ is itself a latent-sized vector), `σ` (the spread/width of each blob).
- **MDN (Mixture Density Network)** = literally "a neural **Network** that outputs the parameters of a mixture-of-Gaussians probability **Density**." Demystified.
- **Sampling** = (1) roll a weighted die using `π` to pick a blob, (2) draw a random point from that Gaussian (`μ`, `σ`). The point drawn **is** the next latent `z'`.
- Analogy: the RNN hands you a **loaded die** (the description of how likely each outcome is); sampling is *rolling* it.

### blob / blurry (why we need a MIXTURE, not one Gaussian)
- **blob** = one Gaussian = a fuzzy cloud of "where the value probably is," centered at `μ`, spreading by `σ`. In 32D you can't picture it, so "blob" is the intuition.
- **blurry** = the failure when you force *one* blob to cover *several* separate futures (enemy at left door OR right door): its center is pushed to the empty middle and smeared wide, putting highest probability on an outcome that never happens. Decoded to an image it literally looks blurry (ghost at both doors).
- A **mixture** fixes this: one crisp blob on the left door, one on the right, "50/50," empty middle stays empty. This is also why naive image predictors produce blurry frames — they predict one averaged outcome.

### controller (C) / policy
- The **policy** = the agent's strategy, a function `state → action`. The **controller C** *is* the policy in the World Models paper.
- Inputs: **`controller(z, h) → action`** — it sees both "what the world looks like now" (`z`) and "the summary of what's been happening" (`h`).
- Deliberately **tiny** (often a single linear layer): the VAE and RNN did the hard representation work; C just reads it and picks a move. The intelligence lives in V and M.

### model-free vs model-based RL
- **model-free**: the agent never builds its own model of the world; it just learns "in state X, action Y tends to pay off" by brute-force trial and error. Works, but sample-hungry (needs a staggering number of tries).
- **model-based**: the agent *learns its own world model* (the `state+action→next state` function), then can **"dream"** — practice on imagined rollouts inside its own head, cheaply, without touching the real environment. (The VizDoom "training inside a hallucinated game" demo.)

### credit assignment problem
Not a function — a *problem*. When you crash 200 steps after a bad turn, *which* of those actions deserves the blame? Assigning credit/blame across a long sequence is hard. (World models help with this.)

### RL vocabulary (the 20-min version — enough, no full course needed)
agent, environment, **state**, **action**, **reward** (a number: +1 good, −1 crash), **return** (total reward over time, future discounted — the thing the agent maximizes), **policy** (state → action).

### Loss vs Reward (cousins, NOT the same number)
- **Loss** = how *wrong* a prediction was (prediction vs recorded truth, self-supervised). We **minimize** it. Used by VAE + RNN. Doesn't care if the outcome was good — only whether you saw it coming.
- **Reward** = how *good* an outcome was for the goal (comes from the task/environment). We **maximize** it. Used by the controller. Needs a notion of "good."
- Frame: the world model learns to be a good **fortune-teller** (minimize loss — predict accurately); the controller learns to be a good **decision-maker** (maximize reward — steer toward good futures). A perfect fortune-teller who makes terrible decisions is entirely possible — that's why they're separate.

---

## Answered questions (from my raw notes)

**Q: Is a world model just a function: current state + action → future state?**
Yes. `world_model(state, action) → next state`. Nailed it. The rest is just how to implement that well for images (VAE compresses, RNN gives memory).

**Q: How did RL work before world models? Did world models exist unnamed?**
Before: RL ran against a hand-coded **RL environment** (or the real world) directly — model-free, brute-force, sample-hungry. And yes — the idea is decades old (Schmidhuber; Sutton's "Dyna," 1991; "learned forward models" in control theory). "World model" is a newly-popular *name* for an old idea that now works at scale thanks to compute + VAE/Transformer machinery.

**Q: How are RL-environment, real-environment, and world-model different?**
See "The three environments" above: real = reality; RL environment = human-built simulator; world model = *learned* self-taught simulator.

**Q: Do I need to learn RL first?**
No. ~20 min of RL vocabulary (above), not a course. RL is a supporting character here; the load-bearing idea (compress → predict next latent) isn't RL. Learning a full RL course now = the top-down trap. Keep going.

**Q: How is the RNN trained? (the piece I couldn't see)**
Self-supervised **next-latent prediction on logged trajectories**, done *before* any acting: (1) let a random agent flail and record the stream z₀,a₀,z₁,a₁,…; (2) feed the RNN (zₜ,aₜ,hₜ), ask it to predict zₜ₊₁, compare to the recorded truth (free label!), backprop the error. No rewards, no acting — just "watch a lot of the world, learn to predict what comes next." The controller is trained separately, last, to maximize reward (and can learn inside dreams).

---

## Corrected mistakes (worth remembering)
- ❌ "RNN produces the next image" → ✅ RNN produces the next **latent** (stays in latent space; only decode to an image when you want to *see* it).
- ❌ "RNN compresses the environment" → ✅ the **VAE** compresses (space/pixels); the **RNN** handles memory + prediction (time). Two components, two jobs.
- ❌ "RNN outputs the next latent directly" → ✅ it outputs distribution **parameters** (π, μ, σ); a separate **sampling** step produces the latent.

---

## Original raw notes (kept as my record)

### 18th Sept
- reinforcement learning environment? — is it same as a world model? → NO (see three-environments above)
- recurrent neural network? → handles time/memory (see RNN above)
- model free RL? → see model-free vs model-based above
- credit assignment problem? — my guess: a function scoring/incentivising an action → close; it's a *problem* (see above)
- backpropagation algorithm? — my guess: measure distance from expected outcome, tune params → right (exact version in Phase 1)
- model based RL? → see above
- dynamics model? → the learned `state+action→next state` function = the RNN/world model proper
- Variational Autoencoder (VAE)? → see above
- stochastic? — my guess: uncertain → right
- Mixture Density Network? → net that outputs mixture-of-Gaussians params (see above)
- Mixture of gaussian distribution? — my Q: is it a vector? → it's params (π,μ,σ) you sample from; not itself the latent
- hidden state of RNN? → running memory / summary of the past (see above)

### Other questions → all answered above (see "Answered questions")

### Learnings (original)
1. ~~Recurrent Neural networks are used to model an environment by compressing certain details based on relevance.~~ → corrected: the VAE compresses; the RNN predicts over time. (see "Corrected mistakes")
