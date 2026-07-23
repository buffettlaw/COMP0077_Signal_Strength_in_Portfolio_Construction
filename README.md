# COMP0077 — Signal Strength in Portfolio Construction

Numerical-simulation code for the MSc Computational Finance dissertation
**"Mathematical Modelling of Signal Strength in Portfolio Construction."**

> **Author:** Ka Shing Law · MSc Computational Finance
> **Module:** COMP0077 — Numerical Simulation

---

## Overview

This project studies how *noisy investment signals* should influence portfolio
weights. It treats classical portfolio construction as points on a continuum
between two limiting cases — an oracle investor (all capital in the single best
name) and a no-information investor (equal weighting across indistinguishable
assets) — and asks what mathematical object that continuum is.

The central object is the **signal–strength map**, a softmax tilt

$$
w_i(\boldsymbol{s}, \beta) \;=\; \frac{e^{\beta s_i}}{\sum_j e^{\beta s_j}},
$$

where `s` encodes per-name views and the scalar `β` (strength) controls how
hard those views are acted upon. As `β → 0` the map recovers equal weighting;
as `β → ∞` it concentrates on the single best-scoring name. The experiments
probe the behaviour in between under controlled Brownian / geometric Brownian
motion (GBM) price processes.

## Repository contents

| File | Description |
|------|-------------|
| `Efficient Frontier.ipynb` | Section 2.1 — Markowitz (1952) mean–variance efficient frontier, derived and plotted from closed-form constants. |
| `softmax_simulation.ipynb` | Chapter 3 — the three numerical experiments on the signal–strength map under GBM. |
| `efficient_frontier.png` | Rendered efficient-frontier figure. |
| `possible_experiments.pdf` | Design notes for the numerical experiments. |
| `README.md` | This file. |

## The experiments (Chapter 3)

Each stock follows GBM, $\mathrm{d}S_i = \mu_i S_i\,\mathrm{d}t + \sigma_i S_i\,\mathrm{d}W_i$,
with a signal $s_i = \mu_i + \varepsilon_i$ of tunable noise. Portfolios are
scored by certainty-equivalent (CE) utility relative to the `β = 0`
equal-weight baseline.

- **Experiment 3.1 — Strength sweep.** Sweep `β` upward and plot expected
  utility. The curve is *non-monotone*: it rises, peaks at an interior optimal
  strength `β*`, then falls as concentration risk overtakes the gain from
  tilting.
- **Experiment 3.2 — Signal quality.** Vary the signal noise (clean → noisy)
  and track `β*`. Noisier signals earn less and command a smaller optimal
  strength, `β* → 0` as the signal becomes pure noise — tracking a Bayesian
  shrinkage benchmark. Includes the clean-versus-noisy overlay separating the
  *vertical* gap (utility lost to noise) from the *horizontal* gap (pull-back
  of the optimal strength).
- **Experiment 3.3 — Correlated assets.** Give the diffusion shocks a common
  pairwise correlation `ρ` (equicorrelation) or a random 3-factor loading
  structure with heterogeneous, partly negative correlations. Correlation destroys
  the value of diversification (the baseline CE level falls) but not the value
  of the signal: the concentration penalty scales with `1 − ρ`, so `β*` and the
  peak ΔCE both *rise* with `ρ`.

## Requirements

- Python 3.10+
- `numpy`, `matplotlib`
- `jupyter` (to run the notebooks)

```bash
pip install numpy matplotlib jupyter
```

## Usage

```bash
jupyter notebook
```

Then open `Efficient Frontier.ipynb` or `softmax_simulation.ipynb` and run
all cells. A fixed RNG seed (`np.random.default_rng(42)`) makes the simulation
results reproducible.

## Methodological notes

The signal lives on the scale of annual drifts (a few percent), so `β` must
reach the order of $1/\sigma_\mu \approx 20$ before the tilt differentiates the
weights; the observed `β*` sits at that scale. The `β` axis in Chapter 3 is
therefore not directly comparable to the illustrative `β ∈ {0, 1, 3}` used in
the Chapter 2 figures.

## References

- Markowitz, H. (1952). *Portfolio Selection.* Journal of Finance.
- Black, F. & Litterman, R. (1992). *Global Portfolio Optimization.*

## Author

Ka Shing Law — MSc Computational Finance
