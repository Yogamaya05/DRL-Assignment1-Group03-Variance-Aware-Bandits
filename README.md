# Fair Evaluation of Variance-Aware Bandit Algorithms: Reproduction and Analysis

**Deep Reinforcement Learning, Assignment 1 · Group 3 · BITS Pilani (WILP)**

This repository contains our implementation and analysis for the paper
**"A Framework for Fair Evaluation of Variance-Aware Bandit Algorithms"** (Wolf, 2025, [arXiv:2510.27001](https://arxiv.org/abs/2510.27001)).

## Team and contributions

All four members studied the paper together and divided the sections equally. Each member presents their own part
in the video, in the order below.

| Member | BITS ID | Slides | Contribution | Related notebook sections |
|---|---|---|---|---|
| Yogamaya Dash | 2025AG05620 | 1–8 | Objective and motivation of the paper; the eight algorithms it evaluates | §2, §3 |
| Suchintak Patnaik | 2025AG05930 | 9–13 | Experimental set-up (10⁶ steps, 100 runs, same seeds, both arm orders); variance of each arm, p(1−p); Scenario A (baseline) | §1, §4, §5 (Scenario A) |
| Dialani Chitra BasantKumar | 2025AG05777 | 14–15 | Scenarios B and C: UCB-Tuned vs plain UCB and tuned ETC; when variance-awareness matters most | §5 (Scenarios B, C) |
| Bhatt Dhruv Anil | 2025AG05557 | 16–25 | Overall findings, limitations, critical reflections, future work, conclusion | §6, §7 |

---

## Purpose

The paper compares eight multi-armed bandit algorithms (classical, variance-aware and elimination-based) on a
two-armed Bernoulli bandit, under a common fairness protocol: the same initialisation, fixed random seeds, one pull
per time step, and every arm order evaluated.

This notebook does three things:

1. **Verifies the numbers in our presentation.** It downloads the author's published result files and checks that every
   regret value on our slides matches them. All 33 match.
2. **Reproduces the experiment independently.** It implements all eight algorithms from their original papers and re-runs
   the paper's three scenarios at full scale: 10⁶ pulls, 100 runs, both arm orders.
3. **Analyses the results critically.** It explains where our numbers differ from the paper's, and it adds an experiment
   that the paper's design could not run.

## Main findings

| | Scenario A (0.80 vs 0.90) | Scenario B (0.895 vs 0.900) | Scenario C (0.890 vs 0.895) |
|---|---|---|---|
| UCB, paper / ours | 238.17 / 253.18 | 1127.17 / 1150.13 | 1172.57 / 1149.60 |
| UCB-Tuned, paper / ours | 30.67 / 35.34 | 212.21 / 221.91 | 226.09 / 231.02 |
| UCB-V, paper / ours | 109.93 / 113.06 | 346.58 / 350.90 | 367.65 / 363.12 |
| EUCBV, paper / ours | 47.73 / 52.56 | 461.36 / 462.12 | 476.31 / 468.67 |
| ETC (m = 100 in A, 10,000 in B/C), paper / ours with a strict commit | 10.10 / 2509.50 | 150.66 / 687.00 | 170.53 / 662.50 |
| UCB-Improved, paper / ours | 32500.11 / 414.51 | 2525.00 / 1806.97 | 2450.00 / 1803.40 |

*All values are mean cumulative regret at T = 10⁶; lower is better.*

* **The UCB-family results reproduce closely.** UCB-Tuned beats plain UCB by about 5× when the arms are hard to tell
  apart (Scenarios B and C), which supports the paper's central claim.
* **Three implementation choices in the author's code shape the paper's ranking.** We implemented each one as an
  extra "author variant", and with them our simulator matches the published numbers (rank correlation ≥ 0.97).
  * **ETC never locks in an arm.** After exploring, it keeps re-checking the arm averages, so a wrong choice can be undone.
    With a true commit, ETC is no longer the best algorithm.
  * **ε-Greedy is order-dependent and unseeded.** It starts from Q = 0 without pulling each arm once, and its exploration
    uses an unseeded random generator. The paper reports only one arm order; in the other order, results differ by up to 8×.
  * **UCB-Improved eliminates arms after one pull per arm per phase.** Each run ends with regret near 0 or near Δ·T, which
    produces the paper's very large value in Scenario A.
* **With textbook algorithms and both arm orders, UCB-Tuned has the lowest mean regret in all three scenarios.**
* **Extension: separating noise from the gap.** With 0/1 rewards the variance is fixed by the win rate, so the paper cannot
  change the noise without also changing the gap. Using Beta-distributed rewards, we keep the gap fixed and vary the
  variance by more than 100×:
  * Plain UCB is unaffected by the change in variance.
  * UCB-V's regret rises about 4× as the noise increases.
  * EUCBV hardly reacts to variance at all.

## Repository structure

```
Code.ipynb                 Main notebook: code, outputs and discussion (run top to bottom)
requirements.txt           Python dependencies
results/
  author_published_results.csv   Author's published regret values for every setting and arm order
  main_results.csv               Summary of our runs (mean, median, VaR, sub-optimal ratio, comparison columns)
  per_run_regret.csv             Final regret of every individual run
  etc_commit_analysis.csv        ETC wrong-commit rates: theory vs our runs vs implied by the paper
  extension_beta_rewards.csv     Variance-decoupling experiment (Beta rewards)
figures/
  reproduction_vs_author.png     Our reproduction vs the author's published values
  scenarios_regret.png           Regret per scenario: reproduction and fair version
  tuning_sensitivity.png         ETC m-sweep and ε-Greedy ε-sweep
  scenarioC_curves_risk.png      Regret over time and per-run distribution (Scenario C)
  bonus_shrink.png               Why variance-aware exploration bonuses shrink for consistent arms
  extension_variance_sweep.png   Regret vs reward variance at a fixed gap
author_data/                     The author's published result CSVs used for verification (MIT licence, see inside)
```

## Notebook contents

| § | Section | Presented by |
|---|---|---|
| 1 | Setup: the three scenarios, and why 0/1 rewards tie the variance to the win rate | Suchintak Patnaik |
| 2 | All eight algorithms, the fair-evaluation simulator, and the author-variant implementations | Yogamaya Dash |
| 3 | Why variance-awareness helps: the exploration-bonus comparison | Yogamaya Dash |
| 4 | Fairness controls: arm ordering and common random numbers | Suchintak Patnaik |
| 5 | Author's published results, our reproduction, our fair version, risk metrics, UCB-Improved evidence | Suchintak Patnaik (Scenario A), Dialani Chitra BasantKumar (Scenarios B, C) |
| 6 | Tuning sensitivity and why ETC's average regret is fragile | Bhatt Dhruv Anil |
| 7 | Extension: decoupling mean and variance with Beta rewards | Bhatt Dhruv Anil |
| 8 | Summary and references | All |

## How to run

```bash
pip install -r requirements.txt
jupyter notebook Code.ipynb        # then Run All
```

The notebook also runs on Google Colab without changes.

* **With `numba`** (recommended), it runs the full paper setting (10⁶ pulls × 100 runs × 2 arm orders × 16 settings ×
  3 scenarios) in about 4 minutes on a laptop.
* **Without `numba`**, it switches automatically to a reduced "quick" setting.
* **Determinism:** results are fully deterministic for a given seed (`SEED = 2025`).
* **Author data:** the author's result files are downloaded on first use and cached in `author_data/`, which is already
  included, so the notebook also runs offline.

## Implementation notes

* **Common random numbers.** For every run, a reward stack `X[arm, j]` (the reward of the j-th pull of each arm) is drawn
  from the seed before the algorithm starts. Every algorithm therefore sees exactly the same rewards.
* **Regret definition.** Regret is the pseudo-regret Σ (μ* − μ_{a_t}), the same definition the paper uses.
* **Constants.** θ, c, b, q, β and ρ are taken from the author's `src/config.py`, so our numbers can be compared directly
  with hers.

## References

1. Wolf (2025). *A Framework for Fair Evaluation of Variance-Aware Bandit Algorithms.* arXiv:2510.27001.
2. Bandit Playground, the author's code and results (tag v1.0-SKILL2025): https://github.com/eelisee/bandit_playground/tree/v1.0-SKILL2025
3. Auer, Cesa-Bianchi & Fischer (2002). Finite-time analysis of the multiarmed bandit problem. *Machine Learning* 47, 235–256.
4. Audibert, Munos & Szepesvári (2009). Exploration–exploitation tradeoff using variance estimates in multi-armed bandits. *Theoretical Computer Science* 410(19), 1876–1902.
5. Auer & Ortner (2010). UCB revisited: Improved regret bounds for the stochastic multi-armed bandit problem. *Periodica Mathematica Hungarica* 61, 55–65.
6. Mukherjee, Naveen, Sudarsanam & Ravindran (2018). Efficient-UCBV: An almost optimal algorithm using variance estimates. *AAAI 2018*.
7. Lattimore & Szepesvári (2020). *Bandit Algorithms.* Cambridge University Press.
8. Sutton & Barto (2018). *Reinforcement Learning: An Introduction*, 2nd ed. MIT Press.
