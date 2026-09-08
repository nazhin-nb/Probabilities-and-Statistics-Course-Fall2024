# Engineering Probability & Statistics — Computer Assignment 3 (CA3)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.20%2B-orange.svg)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.8%2B-navy.svg)](https://scipy.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-green.svg)](https://scikit-learn.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.5%2B-red.svg)](https://matplotlib.org/)
[![Course](https://img.shields.io/badge/Course-Probabilities_%26_Statistics_Fall_2024-purple.svg)](#)

> **Author:** Nazhin Nikkhahbahrami  
> **Student ID:** 810102530  
> **Department:** Electrical & Computer Engineering, University of Tehran  
> **Course:** Engineering Probability & Statistics (Fall 2024)  
> **Instructors:** Dr. Tavassolipour, Dr. Vahabi

---

## 📌 Overview

This repository contains the complete implementation, mathematical derivations, and simulation analyses for **Computer Assignment 3 (CA3)** of the Engineering Probability and Statistics course. The assignment is implemented in **Python** and focuses on empirical and analytical investigations of three core topics:

1. **Central Limit Theorem (CLT) & Asymptotic Normality (`Question1.ipynb`):** Verifying the convergence of sample means to a Gaussian distribution across Poisson, Exponential, and Geometric populations; measuring standard error decay rates ($\propto 1/\sqrt{N}$); and demonstrating the failure of unimodal CLT convergence in non-identically distributed mixture models.
2. **Mean Squared Error (MSE) Optimization & Linear Regression (`Question2-MSE.ipynb`):** Deriving closed-form solutions for univariate linear regression, implementing the multivariate Normal Equation $\theta = (X^T X)^{-1} X^T y$ from scratch, evaluating on the Diabetes benchmark dataset, and determining the optimal geometric center $(c_x, c_y)$ minimizing mean squared error.
3. **De Moivre–Laplace Theorem & Bernoulli Sums (`Question3.ipynb`):** Analyzing standardized Binomial distributions ($n = 270, p = 0.3$), deriving continuous-to-discrete scaling factors via Riemann sum integration, evaluating point probabilities ($k = 55$), and computing continuity-corrected interval probabilities ($P(40 \le S_n \le 60)$) matching exact Binomial sums within $0.05\%$.

---

## 📁 Repository Structure

```text
CA3/
├── EPS_CA3.pdf              # Official assignment description & theoretical guidelines
├── Question1.ipynb          # Central Limit Theorem & mixture distribution simulations
├── Question2-MSE.ipynb      # Complete implementation: Linear regression & dataset center via MSE
├── MSE.ipynb                # Initial assignment template notebook provided by instructors
├── Question3.ipynb          # De Moivre-Laplace CLT, Riemann scaling & interval probabilities
└── README.md                # Project documentation
```

---

## 🔬 Detailed Technical Breakdown

### Question 1: Central Limit Theorem & Mixture Breakdown (`Question1.ipynb`)

- **Theoretical Population Moments:**
  - **Poisson Distribution ($\lambda = 10$):** $\mathbb{E}[X] = 10$, $\text{Var}(X) = 10$, $\sigma \approx 3.162$.
  - **Exponential Distribution ($\lambda = 0.5$):** $\text{Scale} = 1/\lambda = 2 \implies \mathbb{E}[X] = 2$, $\text{Var}(X) = 4$, $\sigma = 2$.
  - **Geometric Distribution ($p = 0.2$):** $\mathbb{E}[X] = 1/p = 5$, $\text{Var}(X) = (1-p)/p^2 = 20$, $\sigma \approx 4.472$.

- **Monte Carlo Sampling & Sample Mean Convergence:**
  - Evaluated sample sizes $N \in \{30, 300, 3000\}$ across $1,000$ independent trials (`seed = 530`).
  - Plotted standardized histograms with uniform bin steps (`0.1`) across all distributions.
  - Demonstrated that the distribution of the sample mean $\bar{X}_N = \frac{1}{N} \sum_{i=1}^N X_i$ rapidly loses the skewness of the underlying distribution and transitions to a symmetric bell curve:
    $$\bar{X}_N \xrightarrow{d} \mathcal{N}\left(\mu, \frac{\sigma^2}{N}\right)$$
  - **Standard Error Decay:** Standard deviations of sample means scale precisely with $1/\sqrt{N}$ (shrinking by approximately $\sqrt{10} \approx 3.16\times$ when $N$ increases tenfold).

- **Mixture Distribution & Failure of Identical Distribution Assumption:**
  - Constructed a stochastic mixture where each experiment selects one of the three populations (Poisson, Exponential, Geometric) with equal probability ($P = 1/3$).
  - Evaluated the distribution of the sample mean across $N \in \{30, 300, 3000\}$.
  - **Key Finding:** As $N$ increases, the mixture does **not** converge to a single Gaussian curve. Instead, it forms a distinct **multimodal (trimodal) distribution** centered around the three individual means ($\mu = 2, 5, 10$).
  - **Theoretical Explanation:** Classical CLT strictly requires random variables to be **independent and identically distributed (i.i.d.)**. Since the mixture switches populations between runs, the identically distributed requirement is violated, yielding a Gaussian mixture rather than a single normal variable.

---

### Question 2: Mean Squared Error (MSE) Optimization & Linear Models (`Question2-MSE.ipynb`)

#### 1. Univariate Simple Linear Regression (`SimpleLinearRegression`)
- **Mathematical Derivation:**
  Given model $y = mx + b$, minimize:
  $$\text{MSE}(m, b) = \frac{1}{n} \sum_{i=1}^n (y_i - (mx_i + b))^2$$
  Setting partial derivatives to zero yields:
  $$m = \frac{\sum_{i=1}^n (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^n (x_i - \bar{x})^2} = \frac{\text{Cov}(x, y)}{\text{Var}(x)}, \quad b = \bar{y} - m\bar{x}$$
- **Evaluation:**
  - Trained on the Diabetes dataset (feature index 2: BMI).
  - Test Mean Squared Error: $\text{MSE} = \mathbf{2548.0724}$.
  - Plotted test data points against the fitted linear model.

#### 2. Multivariate Linear Regression via Normal Equation (`LinearRegression`)
- **Matrix Formulation:**
  For design matrix $X \in \mathbb{R}^{m \times (n+1)}$ (including an explicit intercept bias column $\mathbf{1}$) and target $y \in \mathbb{R}^{m \times 1}$:
  $$\text{MSE}(\theta) = \frac{1}{m} (y - X\theta)^T (y - X\theta)$$
  $$\nabla_\theta \text{MSE}(\theta) = -\frac{2}{m} X^T (y - X\theta) = 0 \implies X^T X \theta = X^T y$$
  $$\theta^* = (X^T X)^{-1} X^T y$$
- **Implementation:**
  - Augmented $X$ with bias column using `np.c_[np.ones(X.shape[0]), X]`.
  - Computed $\theta^*$ using `np.linalg.inv(X.T @ X) @ X.T @ y`.
  - Evaluated on test set: $\text{MSE} = \mathbf{2548.0724}$ (matching analytical optimality).

#### 3. Finding the Dataset Center Minimizing MSE
- **Problem Formulation:**
  Find $(c_x, c_y)$ minimizing the sum of squared Euclidean distances:
  $$\text{MSE}_{\text{center}}(c_x, c_y) = \frac{1}{n} \sum_{i=1}^n \left[(x_i - c_x)^2 + (y_i - c_y)^2\right]$$
  Setting partial derivatives to zero:
  $$\frac{\partial \text{MSE}}{\partial c_x} = -\frac{2}{n} \sum_{i=1}^n (x_i - c_x) = 0 \implies c_x^* = \frac{1}{n} \sum_{i=1}^n x_i = \bar{x}$$
  $$\frac{\partial \text{MSE}}{\partial c_y} = -\frac{2}{n} \sum_{i=1}^n (y_i - c_y) = 0 \implies c_y^* = \frac{1}{n} \sum_{i=1}^n y_i = \bar{y}$$
- **Result:**
  - Computed Center Coordinates: $(c_x^*, c_y^*) = (\mathbf{0.000473}, \, \mathbf{153.3626})$.
  - Visualized data scatter overlaid with optimal center marker (red `X`).

---

### Question 3: De Moivre–Laplace Theorem & Bernoulli Convergence (`Question3.ipynb`)

- **Setup & Standardization:**
  Consider $S_n \sim \text{Binomial}(n = 270, p = 0.3)$:
  $$\mu = np = 81.0, \quad \sigma = \sqrt{np(1-p)} = \sqrt{270 \cdot 0.3 \cdot 0.7} = \sqrt{56.7} \approx \mathbf{7.5299}$$
  Standardized variable: $Z_n = \frac{S_n - 81.0}{7.5299}$.

- **Continuous-to-Discrete Scaling via Riemann Sum:**
  - The sum of discrete Binomial PMF bars equals $\sum_{k=0}^{270} P(X = k) = \mathbf{1.0}$.
  - Because discrete bar widths in standardized space equal $\Delta z = \frac{1}{\sigma}$, direct height comparisons between discrete PMF and continuous Gaussian PDF require scaling:
    $$\text{Scale Factor} \approx \frac{1}{\sigma} = \frac{1}{\sqrt{np(1-p)}} \approx \frac{1}{7.5299} \approx 0.1328$$
  - Numerically validated via Riemann sum integration across $[\mu - 4\sigma, \mu + 4\sigma]$ ($N = 1000$ points), yielding scaled alignment between the continuous curve and discrete PMF bars.

- **Point Probability Estimation ($k = 55$):**
  - **Exact Binomial Probability:**
    $$P(S_{270} = 55) = \binom{270}{55} (0.3)^{55} (0.7)^{215} \approx \mathbf{0.0001}$$
  - **Gaussian Density Approximation:**
    $$\frac{1}{\sigma} \phi\left(\frac{55 - 81}{7.5299}\right) \approx \mathbf{0.0010}$$

- **Interval Probability with Continuity Correction ($n = 100, p = 0.5$):**
  - Evaluating $P(40 \le S_{100} \le 60)$ where $\mu = 50$, $\sigma = \sqrt{100 \cdot 0.25} = 5.0$:
  - **Exact Cumulative Binomial Sum:**
    $$\sum_{k=40}^{60} \binom{100}{k} (0.5)^{100} = \mathbf{0.9647998}$$
  - **Continuity-Corrected Normal CDF Approximation:**
    $$\Phi\left(\frac{60.5 - 50}{5}\right) - \Phi\left(\frac{39.5 - 50}{5}\right) = \Phi(2.1) - \Phi(-2.1) = \mathbf{0.9642712}$$
  - **Discrepancy:** The approximation error is less than **$0.05\%$** ($\Delta = 5.28 \times 10^{-4}$).

---

## 📊 Summary of Quantitative Results

| Experiment | Metric / Target | Computed Result | Theoretical Target | Relative Error |
| :--- | :--- | :--- | :--- | :--- |
| **Q1: CLT Scaling** | SE Decay ($N = 30 \to 3000$) | $\approx 10\times$ reduction | $\sqrt{3000/30} = 10.0$ | Exact match |
| **Q1: Mixture Model** | Distribution Shape ($N = 3000$) | Trimodal ($\mu \in \{2, 5, 10\}$) | Non-Gaussian mixture | Violation of i.i.d. |
| **Q2: Linear Regression** | Test MSE (Univariate) | $\mathbf{2548.0724}$ | Analytical OLS optimum | $< 10^{-6}$ |
| **Q2: Matrix Regression** | Test MSE (Normal Eq.) | $\mathbf{2548.0724}$ | Analytical OLS optimum | $< 10^{-6}$ |
| **Q2: Optimal Center** | Minimum MSE Center | $(\mathbf{0.000473}, \mathbf{153.3626})$ | Empirical centroid $(\bar{x}, \bar{y})$ | Exact |
| **Q3: Interval Prob** | Exact Binomial ($40 \le X \le 60$) | $\mathbf{0.9647998}$ | Closed-form sum | Reference |
| **Q3: Interval Prob** | Corrected Normal Approx | $\mathbf{0.9642712}$ | $\Phi(2.1) - \Phi(-2.1)$ | **$\Delta = 0.000528$** |

---

## 💻 Environment & Setup

### Prerequisites

- Python 3.8+
- Jupyter Notebook / JupyterLab

### Dependencies

Install the necessary scientific libraries:

```bash
pip install numpy scipy scikit-learn matplotlib
```

### Running the Notebooks

1. **Central Limit Theorem Simulations:**
   ```bash
   cd CA3
   jupyter notebook Question1.ipynb
   ```
2. **Linear Regression & MSE Optimization:**
   ```bash
   cd CA3
   jupyter notebook Question2-MSE.ipynb
   ```
3. **De Moivre–Laplace Binomial Approximations:**
   ```bash
   cd CA3
   jupyter notebook Question3.ipynb
   ```
