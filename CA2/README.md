# Engineering Probability & Statistics — Computer Assignment 2 (CA2)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![SimPy](https://img.shields.io/badge/SimPy-4.x-brightgreen.svg)](https://simpy.readthedocs.io/)
[![SciPy](https://img.shields.io/badge/SciPy-1.8%2B-navy.svg)](https://scipy.org/)
[![Pandas](https://img.shields.io/badge/Pandas-1.4%2B-darkblue.svg)](https://pandas.pydata.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12%2B-lightblue.svg)](https://seaborn.pydata.org/)
[![Course](https://img.shields.io/badge/Course-Probabilities_%26_Statistics_Fall_2024-purple.svg)](#)

> **Author:** Nazhin Nikkhahbahrami  
> **Student ID:** 810102530  
> **Department:** Electrical & Computer Engineering, University of Tehran  
> **Course:** Engineering Probability & Statistics (Fall 2024)  
> **Instructors:** Dr. Tavassolipour, Dr. Vahabi

---

## 📌 Overview

This repository contains the complete implementation and empirical analysis for **Computer Assignment 2 (CA2)** of the Engineering Probability and Statistics course. The project is implemented in **Python** and investigates three foundational branches of applied probability, Bayesian inference, and exploratory data analysis:

1. **Joint Distributions & Queueing System Simulation (`pmf.ipynb`):** Discrete-event simulation of an $M/M/1$ queue using SimPy, Kernel Density Estimation (KDE) bandwidth tuning, bivariate joint distributions (hexbin & scatter plots), total system time convolution, and conditional waiting time distributions.
2. **Bayesian Parameter Estimation (`coin.ipynb`):** Conjugate Beta-Binomial Bayesian framework for estimating the true probability of heads/tails, log-gamma normalization (`scipy.special.gammaln`), sequential posterior updating across 1,000 coin flips, and comparing uninformative Uniform priors $\text{Beta}(1, 1)$ against informative priors $\text{Beta}(4, 10)$.
3. **Correlation, Time-Series & Causal Inference (`energy.ipynb`):** Implementing a custom covariance and Pearson correlation engine from scratch (without built-in `.corr()`), analyzing temporal electricity load patterns (hourly, monthly, and yearly dynamics), seasonal correlation shifts, and investigating spurious correlation vs. causality on socio-demographic indicators (`TV_LE_Physician.csv`).

---

## 📁 Repository Structure

```text
CA2/
├── data/
│   ├── coin_flips.txt          # Sequence of 1,000 coin flips ('H' and 'T')
│   ├── energy.csv              # Hourly energy consumption dataset (AEP_MW)
│   └── TV_LE_Physician.csv     # Life expectancy, physician density & TV ownership dataset
├── coin.ipynb                  # Conjugate Beta-Binomial Bayesian parameter estimation
├── energy.ipynb                # Correlation engine, energy time-series & causal analysis
├── pmf.ipynb                   # SimPy queue simulation & bivariate joint distribution analysis
├── correlation_matrix.csv      # Exported Pearson correlation matrix from custom engine
├── EPS_CA2.pdf                 # Assignment problem description
└── README.md                   # Project documentation
```

---

## 🔬 Detailed Technical Breakdown

### Question 1: Joint Distributions & Discrete-Event Queue Simulation (`pmf.ipynb`)

- **Simulation Architecture:**
  - Implemented an $M/M/1$ single-server queueing simulation using **SimPy**.
  - Inter-arrival times and service durations are modeled as independent exponential random variables.
  - Recorded individual trajectories for `arrival_times`, `wait_times`, and `service_times`.

- **KDE Bandwidth Tuning (`bw_adjust` Sensitivity):**
  - Explored the bias-variance tradeoff in Kernel Density Estimation by comparing bandwidth scaling factors: `bw_adjust = 1`, `bw_adjust = 5`, and `bw_adjust = 10`.
  - **Observation:** `bw_adjust = 1` accurately preserves local modality and high-density peaks near zero, whereas larger bandwidths ($5, 10$) excessively smooth the density, masking the true distribution geometry.

- **Bivariate Joint Distributions & Correlation:**
  - Plotted bivariate joint distributions using hexagonal binning (`sns.jointplot(kind="hex")`) and scatter plots.
  - **Arrival vs. Service Times:** Exhibits circular/uniform spreading, verifying statistical independence ($\text{Cov}(T_{\text{arr}}, T_{\text{serv}}) \approx 0$).
  - **Arrival vs. Wait Times:** Depicts cumulative queue congestion dynamics over the simulation horizon.

- **Total System Time & Convolution:**
  - Computed total time in system: $T_{\text{total}} = T_{\text{wait}} + T_{\text{service}}$.
  - The distribution corresponds to the convolution of waiting and service time distributions, exhibiting a long-tailed exponential decay.

- **Conditional Distribution:**
  - Evaluated conditioned wait times given early customer arrivals ($\text{Arrival Time} < 50$) to analyze queue startup/transient characteristics versus steady-state operation.

---

### Question 2: Conjugate Beta-Binomial Bayesian Parameter Estimation (`coin.ipynb`)

- **Mathematical Model:**
  Estimating the bias parameter $\theta = P(\text{Heads})$ from $N = 1000$ sequential coin flips (`365 Heads`, `635 Tails`).
  Under a conjugate $\text{Beta}(\alpha, \beta)$ prior:
  $$\text{Prior: } p(\theta) = \frac{1}{B(\alpha, \beta)} \theta^{\alpha - 1} (1 - \theta)^{\beta - 1}$$
  $$\text{Likelihood for } k \text{ Heads, } m \text{ Tails: } P(D \mid \theta) = \binom{k+m}{k} \theta^k (1 - \theta)^m$$
  $$\text{Posterior: } p(\theta \mid D) = \text{Beta}(\alpha + k, \, \beta + m)$$

- **Numerically Stable Beta Implementation:**
  - Developed custom `BetaDistribution` class.
  - Avoided arithmetic overflow by computing the normalization constant in log-space via `scipy.special.gammaln`:
    $$B(\alpha, \beta) = \exp\left(\ln\Gamma(\alpha) + \ln\Gamma(\beta) - \ln\Gamma(\alpha + \beta)\right)$$

- **Sequential Posterior Updating (Every 50 Trials):**
  - Tracked posterior progression across 20 intermediate checkpoints ($4 \times 5$ subplot grid).
  - **Prior 1 — Non-informative Uniform ($\text{Beta}(1, 1)$):**
    - Initial: Flat uniform density over $[0, 1]$.
    - Final Posterior Mean: $\mathbf{0.3683}$
    - Final Posterior Variance: $\mathbf{0.1359}$ (custom moment implementation)
  - **Prior 2 — Informative Prior ($\text{Beta}(4, 10)$):**
    - Initial: Right-skewed density biased toward $\approx 0.285$.
    - Final Posterior Mean: $\mathbf{0.3668}$
    - Final Posterior Variance: $\mathbf{0.0002332}$

- **Bayesian Asymptotic Convergence (Bernstein–von Mises Theorem):**
  - Both posteriors rapidly converge to the true empirical frequency ($\approx 0.365$).
  - As $N \to 1000$, the sample data overwhelmingly dominates the prior belief, proving that the choice of prior becomes negligible in large-sample regimes.

---

### Question 3: Time-Series Correlation, Seasonality & Causal Inference (`energy.ipynb`)

- **Custom Pearson Correlation Engine:**
  - Implemented the `correlation(x: pd.DataFrame)` function from first principles without using Pandas `.corr()`:
    $$\text{Cov}(X, Y) = \frac{1}{n - 1} \sum_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y}), \quad r_{XY} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}$$

- **Energy Consumption Analysis (`energy.csv`):**
  - **Temporal Feature Extraction:** Extracted `Year`, `Month`, and `Hour` from timestamps.
  - **Yearly Dynamics:** Boxplots across years revealed heightened dispersion in 2005.
    - Sample Variance in 2004: $\mathbf{4,312,554.64}$
    - Sample Variance in 2005: $\mathbf{6,609,516.55}$ (significantly higher load fluctuation).
  - **Diurnal Cycle:** Hourly boxplots indicate minimal consumption in early morning ($04:00$), rising sharply during operational hours.
    - Correlation between Morning Hours ($04:00 - 13:00$) and Energy Usage: $r = \mathbf{+0.4714}$ (steady morning ramp-up).
  - **Seasonal Cycle:**
    - Late Winter / Spring ($02 \le \text{Month} \le 04$): $r = \mathbf{-0.5582}$ (drop in heating demand as weather warms).
    - Autumn / Early Winter ($10 \le \text{Month} \le 12$): $r = \mathbf{+0.4738}$ (surge in energy demand as temperatures drop).

- **Causal Effect vs. Spurious Correlation (`TV_LE_Physician.csv`):**
  - Exported correlation matrix to [`correlation_matrix.csv`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA2/correlation_matrix.csv):
    | Variables | Life Expectancy | Physicians per 1k | TVs per 1k |
    | :--- | :---: | :---: | :---: |
    | **Life Expectancy (years)** | $1.0000$ | **$0.6288$** | $0.0259$ |
    | **Physicians per 1000 people** | **$0.6288$** | $1.0000$ | $0.0086$ |
    | **Televisions per 1000 people** | $0.0259$ | $0.0086$ | $1.0000$ |
  - **Causal Analysis:**
    - **Physicians vs. Life Expectancy ($r \approx +0.629$):** Strong positive correlation backed by direct causal medical mechanisms (healthcare access increases lifespan).
    - **Televisions vs. Life Expectancy ($r \approx +0.026$):** Negligible linear correlation; high TV counts serve as a loose proxy for national GDP/wealth (confounding variable), not a causal determinant of longevity.
    - **Conclusion:** Correlation does not imply causation ($X \not\to Y$ merely because $\text{Corr}(X, Y) \ne 0$).

---

## 📊 Summary of Quantitative Results

| Module | Metric / Experiment | Computed Value | Theoretical / Physical Meaning |
| :--- | :--- | :--- | :--- |
| **coin.ipynb** | Total Coin Flips | $1000$ ($365$ Heads, $635$ Tails) | Ground truth head bias $\approx 0.365$ |
| **coin.ipynb** | Posterior Mean ($\text{Beta}(1, 1)$) | $\mathbf{0.3683}$ | Data-driven posterior estimate |
| **coin.ipynb** | Posterior Mean ($\text{Beta}(4, 10)$) | $\mathbf{0.3668}$ | Invariant under large $N$ |
| **energy.ipynb** | Load Variance (2004 vs 2005) | $4.31 \times 10^6 \to \mathbf{6.61 \times 10^6}$ | Significant increase in 2005 volatility |
| **energy.ipynb** | Morning Hours ($04-13$) vs Load | $r = \mathbf{+0.4714}$ | Strong daytime load ramp |
| **energy.ipynb** | Spring Months ($02-04$) vs Load | $r = \mathbf{-0.5582}$ | Heating demand reduction |
| **energy.ipynb** | Winter Months ($10-12$) vs Load | $r = \mathbf{+0.4738}$ | Cold weather heating surge |
| **energy.ipynb** | Physician vs Life Expectancy | $r = \mathbf{+0.6288}$ | Significant positive healthcare link |
| **energy.ipynb** | TV Ownership vs Life Expectancy | $r = \mathbf{+0.0259}$ | Spurious / non-causal relationship |

---

## 💻 Environment & Setup

### Prerequisites

- Python 3.8+
- Jupyter Notebook / JupyterLab

### Dependencies

Install the required libraries:

```bash
pip install simpy numpy scipy pandas matplotlib seaborn
```

### Running the Notebooks

1. **Queue Simulation & Joint Distributions:**
   ```bash
   cd CA2
   jupyter notebook pmf.ipynb
   ```
2. **Bayesian Beta-Binomial Estimation:**
   ```bash
   cd CA2
   jupyter notebook coin.ipynb
   ```
3. **Correlation, Energy Load & Causality:**
   ```bash
   cd CA2
   jupyter notebook energy.ipynb
   ```
