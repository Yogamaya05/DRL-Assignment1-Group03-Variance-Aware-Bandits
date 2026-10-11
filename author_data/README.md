# Author's published results

The CSV files in this folder are copied unchanged from the paper author's repository, **Bandit Playground**:
https://github.com/eelisee/bandit_playground/tree/v1.0-SKILL2025 (folder `data/`).

`Code.ipynb` (§5.0) uses them to verify the numbers in our presentation and to compare them with our reproduction.
They are downloaded automatically if this folder is missing.

* `average_results_<p1>_<p2>.csv`: regret averaged over 100 runs at each recorded time step. The suffix gives the arm
  order; for example, `800_900` means arm 1 = 0.80 and arm 2 = 0.90.
* `results_<p1>_<p2>.csv`: one row per run and time step. Only the file used in our analysis (UCB-Improved, Scenario A) is included.

These files are distributed under the MIT License, Copyright (c) 2025 eelisee. See `LICENSE`.
