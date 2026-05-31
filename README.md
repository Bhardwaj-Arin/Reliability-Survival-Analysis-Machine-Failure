# Reliability & Survival Analysis of Industrial Equipment
### Statistical Study of Machine Failure Patterns using the AI4I 2020 Dataset

---

## Objective

To apply reliability theory and statistical inference on real industrial failure data — studying failure lifetime distributions, empirical survival functions, and hypothesis testing on machine sensor data.

This project is motivated by research on **Statistical Inference for Lifetime Distributions** (Kayal et al., 2019–2025), with the goal of empirically validating theoretical reliability concepts on a publicly available industrial dataset.

---

## Dataset

**AI4I 2020 Predictive Maintenance Dataset** — sourced from Kaggle

- 10,000 machine observations with 14 features
- Records sensor readings: Air Temperature, Process Temperature, Rotational Speed, Torque, and Tool Wear
- Labels 5 failure modes: TWF, HDF, PWF, OSF, RNF
- Binary target: `Machine failure` (0 = working, 1 = failed)

---

## Tools & Libraries

| Tool | Purpose |
|---|---|
| Python | Core language |
| Pandas | Data loading and manipulation |
| NumPy | Numerical computations |
| Matplotlib & Seaborn | Visualizations |
| Scipy (stats) | Distribution fitting, KS test, T-test, Chi-square |

---

## Project Structure

```
Predictive_Maintenance_Project.ipynb   ← Main analysis notebook
datasets/
  ai4i2020.csv                         ← Dataset (from Kaggle)
README.md                              ← This file
```

---

## Key Results

### 1. Dataset Overview
- **Total machines observed:** 10,000
- **Total failures:** 339
- **Overall failure rate:** 3.39% — highly imbalanced, realistic of real industry
- **Mean tool wear:** 107.95 min | **Max tool wear:** 253 min

### 2. Failure Type Breakdown

| Failure Type | Count | % of All Failures |
|---|---|---|
| Heat Dissipation Failure (HDF) | 115 | 33.9% |
| Overstrain Failure (OSF) | 98 | 28.9% |
| Power Failure (PWF) | 95 | 28.0% |
| Tool Wear Failure (TWF) | 46 | 13.6% |
| Random Failure (RNF) | 19 | 5.6% |

> HDF dominates, while RNF is extremely rare — consistent with the **bathtub curve** in reliability engineering, where random failures occur only during the useful life phase.

### 3. Distribution Fitting (Kolmogorov-Smirnov Test)

Three classical reliability distributions were fitted to the failure lifetime data (tool wear at failure):

| Distribution | KS Statistic |
|---|---|
| Exponential | 0.2087 |
| **Weibull** | **0.1813 ✅ Best Fit** |
| Log-Normal | 0.1918 |

**Best fitting distribution: Weibull** (lowest KS statistic = 0.1813)

- Weibull shape parameter **β = 1.95 > 1** → confirms **wear-out failure behaviour**
- This means failure risk increases with time — machines become more likely to fail as tool wear accumulates
- Consistent with reliability theory: β > 1 indicates the wear-out phase of the bathtub curve

### 4. Survival Analysis — Empirical Reliability Function R(t)

R(t) = Probability that a machine survives beyond tool wear time t

| Reliability Metric | Tool Wear Time |
|---|---|
| **B10 Life** (10% failure point) | **195 minutes** |
| **Median Life** (50% failure point) | **108 minutes** |
| **B90 Life** (90% failure point) | **20 minutes** |

> **Practical Interpretation:** Maintenance should be scheduled before 195 minutes of tool wear accumulation to keep failure probability below 10%. By 108 minutes, half of all machines have failed.

### 5. Hypothesis Testing

**Test 1 — Chi-Square: Does failure rate differ by machine type (L/M/H)?**
- Chi-square statistic: **13.7517**
- P-value: **0.001032**
- Degrees of freedom: 2
- ✅ **Result: Failure rate SIGNIFICANTLY differs by machine type** (p < 0.05)

**Test 2 — Independent T-Test: Is tool wear higher in failed machines?**
- Mean tool wear (Failed machines): **143.78 min**
- Mean tool wear (Working machines): **106.69 min**
- T-statistic: **10.6029**
- P-value: **< 0.000001**
- ✅ **Result: Failed machines have significantly higher tool wear** (p < 0.05)

### 6. Correlation Analysis

Key findings from the correlation heatmap:
- **Air Temperature ↔ Process Temperature: 0.88** — strong positive correlation (they move together)
- **Rotational Speed ↔ Torque: −0.88** — strong negative correlation (higher speed = lower torque, consistent with physics)
- **Torque ↔ Machine Failure: 0.19** — highest individual correlation with failure
- **Tool Wear ↔ Machine Failure: 0.11** — moderate positive correlation

---

## Key Findings & Conclusions

1. **Weibull distribution** best describes the failure lifetime data (KS = 0.1813), with shape parameter β = 1.95 confirming wear-out failure behaviour — failure risk increases over time.

2. **B10 life = 195 minutes** — maintenance should be scheduled before this threshold to keep failure probability under 10%.

3. **Median machine lifetime = 108 minutes** of tool wear — 50% of machines fail by this point.

4. **Failure rate significantly differs by machine type** (Chi-square, p = 0.001032) — machine quality variants (L/M/H) have different failure sensitivities.

5. **Failed machines have statistically higher tool wear** (T-test, p < 0.05) — tool wear is a significant predictor of failure, though not sufficient alone.

6. **Torque shows the strongest individual correlation (0.19) with machine failure** — mechanical overload is a key failure driver alongside tool wear.

---

## Connection to Reliability Theory

This analysis empirically demonstrates:
- **Lifetime distribution fitting** (Weibull, Log-Normal, Exponential) using MLE + KS test
- **Survival/Reliability function R(t)** and its industrial interpretation (B10, B50 life)
- **Statistical inference on failure data** as studied theoretically in Kayal et al. (2019–2025)

---

## Future Scope

- MLE estimation for Chen/Burr XII distributions on this dataset
- Progressive censoring simulation (right-censored lifetime data)
- Stress-Strength reliability modelling: P(X < Y) estimation
- Bayesian reliability estimation with prior distributions
- Hazard function h(t) estimation and plotting

---

## Dataset Source

Matzka, S. (2020). *AI4I 2020 Predictive Maintenance Dataset*. UCI Machine Learning Repository / Kaggle.  
Link: https://www.kaggle.com/datasets/stephanmatzka/predictive-maintenance-dataset-ai4i-2020
