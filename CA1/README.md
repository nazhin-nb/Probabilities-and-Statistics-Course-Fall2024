# Engineering Probability & Statistics — Computer Assignment 1 (CA1)

[![R](https://img.shields.io/badge/Language-R-276DC3.svg)](https://www.r-project.org/)
[![Jupyter](https://img.shields.io/badge/Notebook-IRkernel-orange.svg)](https://irkernel.github.io/)
[![Probability](https://img.shields.io/badge/Topic-Probability_%26_Distributions-blue.svg)](#)
[![Course](https://img.shields.io/badge/Course-Probabilities_%26_Statistics_Fall_2024-purple.svg)](#)

> **Author:** Nazhin Nikkhahbahrami  
> **Student ID:** 810102530  
> **Department:** Electrical & Computer Engineering, University of Tehran  
> **Course:** Engineering Probability & Statistics (Fall 2024)  
> **Instructor:** Dr. Vahabi

---

## 📌 Overview

This repository contains the complete implementation and technical documentation for **Computer Assignment 1 (CA1)** of the Engineering Probability and Statistics course. The assignment is implemented in the **R programming language** (via `IRkernel` Jupyter Notebooks) and focuses on empirical simulations, asymptotic convergence proofs, and random variable transformation techniques:

1. **Hypergeometric Distribution & Binomial Convergence:** Simulating election audit sampling without replacement, verifying the Law of Large Numbers (LLN) for sample moments, and demonstrating asymptotic convergence to the Binomial distribution as population size $N \to \infty$.
2. **De Moivre–Laplace Theorem & Continuity Correction:** Approximating high-dimensional Binomial distributions with the Normal distribution, and quantitatively proving the significant error reduction achieved via continuity correction ($+0.5$).
3. **Queue Waiting Times & Memoryless Property:** Modeling customer arrival intervals with the Exponential distribution ($\lambda = 1/15$), verifying its memoryless property through stratified simulations, and matching simulated conditional probabilities against analytical limits.
4. **Random Variable Transformations & Box-Muller Algorithm:** Generating continuous random variables via inverse transform sampling ($U(0,1) \to \text{Exp}(0.5)$) and implementing the Box-Muller transform to generate independent Standard Normal variates $\mathcal{N}(0, 1)$ ($10^6$ Monte Carlo samples).

---

## 📁 Repository Structure

```text
CA1/
├── CA1-Nikkhahbahrami-810102530.pdf  # Detailed Persian report with analytical derivations & plots
├── EPS_CA1.pdf                       # Official assignment description & problem statements
├── Q1.ipynb                          # Hypergeometric modeling & Binomial convergence in R
├── Q2.ipynb                          # Normal approximation & continuity correction in R
├── Q3.ipynb                          # Exponential distribution & memoryless simulation in R
├── Q4.ipynb                          # Box-Muller transform & random variable generation in R
└── README.md                         # Project documentation
```

---

## 🔬 Detailed Technical Breakdown

### Question 1: Hypergeometric Distribution, Audit Modeling & Binomial Convergence (`Q1.ipynb`)

- **Context & Formulation:**
  An election auditing board inspects $m = 40$ voting precincts chosen uniformly at random without replacement from a total of $N = 100$ precincts, where $k = 20$ precincts contain election fraud. The number of detected fraudulent precincts follows a Hypergeometric distribution:
  $$P(X = x) = \frac{\binom{k}{x} \binom{N-k}{m-x}}{\binom{N}{m}}, \quad x \in [\max(0, m-(N-k)), \min(k, m)]$$

- **Theoretical Moments:**
  - **Theoretical Mean:** $\mathbb{E}[X] = m \cdot \frac{k}{N} = 40 \cdot \frac{20}{100} = \mathbf{8.0}$
  - **Theoretical Variance:** $\text{Var}(X) = m \cdot \frac{k}{N} \cdot \left(1 - \frac{k}{N}\right) \cdot \left(\frac{N-m}{N-1}\right) = 40 \cdot 0.2 \cdot 0.8 \cdot \frac{60}{99} \approx \mathbf{3.8788}$

- **Simulation & Convergence Analysis:**
  - Evaluated sample sizes $n \in [100, 10000]$ (step size 50) using `rhyper`.
  - Plotted simulated vs. theoretical curves, demonstrating rapid empirical convergence to theoretical moments as $n$ increases.
  - Examined the impact of increasing the audited precinct count $m \in \{40, 60, 80, 100\}$ at $n = 1000$, showing rightward density shifts and higher detection certainty.

- **Convergence to Binomial Distribution:**
  - Implemented custom PMF functions from scratch in R (`Hy_Geo_pmf` and `Bin_pmf`).
  - Tested across increasing population scales $N \in \{100, 500, 1000, 10000\}$ with fixed ratio $p = k/N = 20/N$.
  - Demonstrated that as $N \to \infty$, the distinction between sampling with replacement (Binomial) and without replacement (Hypergeometric) vanishes:
    $$\lim_{N \to \infty} \frac{\binom{k}{x}\binom{N-k}{m-x}}{\binom{N}{m}} = \binom{m}{x} p^x (1-p)^{m-x}$$

---

### Question 2: Normal Approximation to Binomial & Continuity Correction (`Q2.ipynb`)

- **Problem Setup:**
  Consider a Binomial random variable $X \sim B(n=1000, p=0.45)$.
  - Mean: $\mu = n \cdot p = 450$
  - Standard Deviation: $\sigma = \sqrt{n \cdot p \cdot (1-p)} = \sqrt{1000 \cdot 0.45 \cdot 0.55} \approx \mathbf{15.7321}$

- **Comparison of Approximations ($X \le 430$):**
  - **Exact Binomial Cumulative Probability:**
    $$P(X \le 430) = \sum_{x=0}^{430} \binom{1000}{x} (0.45)^x (0.55)^{1000-x} \approx \mathbf{0.1074638}$$
  - **Standard Normal Approximation (Without Correction):**
    $$P(X \le 430) \approx \Phi\left(\frac{430 - 450}{15.7321}\right) = \Phi(-1.2713) \approx \mathbf{0.1018139} \implies \text{Error} = \mathbf{5.65 \times 10^{-3}}$$
  - **Normal Approximation With Continuity Correction ($+0.5$):**
    $$P(X \le 430.5) \approx \Phi\left(\frac{430.5 - 450}{15.7321}\right) = \Phi(-1.2395) \approx \mathbf{0.1075799} \implies \text{Error} = \mathbf{1.16 \times 10^{-4}}$$
  - **Result:** Continuity correction reduces the approximation error by nearly **48-fold**.

- **Global Error Analysis & CDF Comparison:**
  - Evaluated absolute errors across all possible outcomes $X \in [0, 1000]$.
  - Plotted detailed CDF curves in the critical transition range $X \in [440, 460]$, showing near-perfect superposition between the Binomial CDF and the continuity-corrected Normal CDF.

---

### Question 3: Queue Waiting Times & Memoryless Property (`Q3.ipynb`)

- **Context & Modeling:**
  Customer arrivals follow a Poisson process, meaning inter-arrival times follow an Exponential distribution with rate $\lambda = \frac{1}{15} \text{ min}^{-1}$ (average arrival every 15 minutes), monitored over an 8-hour shift ($480 \text{ min}$).

- **Memoryless Property Formulation:**
  For any continuous random variable with the memoryless property:
  $$P(X > s + t \mid X > s) = P(X > t)$$
  Given that a customer has already not arrived for $s = 12$ minutes, the probability distribution of the remaining wait time $T = X - 12$ is identical to the original Exponential distribution $\text{Exp}(\lambda = 1/15)$.

- **Empirical Simulation:**
  - Implemented `exponential_distribution(n, lambda)` and filtered wait intervals with `Time(exp_distribution, arrival=12, maxtime=480)`.
  - Tested across sample sizes $M \times 100$ for $M \in \{10, 100, 1000\}$.
  - Dual histogram plots verified that the conditioned wait time distribution aligns with the original density curve $\lambda e^{-\lambda x}$ with mean $\approx 15 \text{ min}$.

- **Probability Validation ($P(X \le 15 \mid X > 12)$):**
  - **Theoretical Value:**
    $$P(X \le 15 \mid X > 12) = 1 - e^{-\lambda (15 - 12)} = 1 - e^{-\frac{3}{15}} = 1 - e^{-0.2} \approx \mathbf{0.1812692}$$
  - **Simulated Value ($10^5$ iterations):** $\mathbf{0.1812429}$
  - **Accuracy:** The simulation matches the theoretical probability up to 5 decimal places ($\Delta < 2.6 \times 10^{-5}$).

---

### Question 4: Variable Transformations & Box-Muller Algorithm (`Q4.ipynb`)

#### Part 1–3: Uniform to Exponential Transformation
- Generated $n = 10^6$ samples from a Uniform distribution $X \sim U(0, 1)$ (`set.seed(530)`).
- Applied transformation: $Y = -2 \ln(X)$.
- **Analytical CDF & PDF Derivation:**
  $$F_Y(y) = P(-2 \ln X \le y) = P\left(X \ge e^{-y/2}\right) = 1 - e^{-y/2}$$
  $$f_Y(y) = \frac{d}{dy} F_Y(y) = \frac{1}{2} e^{-y/2} \quad (y \ge 0)$$
  This analytically represents an **Exponential distribution** with rate $\lambda = 0.5$ (equivalent to a Chi-Square distribution $\chi^2_2$).
- Verified via histogram with theoretical overlay `curve(0.5 * exp(-x/2))`, demonstrating exact alignment.

#### Part 4–6: The Box-Muller Transformation
- Generated two independent uniform variates $U_1, U_2 \sim U(0, 1)$ ($n = 10^6$).
- Applied polar coordinate transformation:
  $$R = \sqrt{-2 \ln(U_1)}, \quad \theta = 2\pi U_2$$
  $$Z_1 = R \cos(\theta) = \sqrt{-2 \ln(U_1)} \cos(2\pi U_2)$$
  $$Z_2 = R \sin(\theta) = \sqrt{-2 \ln(U_1)} \sin(2\pi U_2)$$
- Plotted empirical probability densities of $Z_1$ and $Z_2$ against the standard normal density $\mathcal{N}(0, 1)$ (`dnorm(x)`).
- Confirmed that the resulting variables are independent, identically distributed standard normals ($Z_1, Z_2 \sim \text{i.i.d. } \mathcal{N}(0, 1)$).

---

## 📊 Summary of Quantitative Results

| Experiment | Metric / Scenario | Simulated / Calculated | Theoretical Value | Error / Discrepancy |
| :--- | :--- | :--- | :--- | :--- |
| **Q1: Hypergeometric** | Expected Mean ($\mathbb{E}[X]$) | $\approx 8.000$ (as $n \to 10^4$) | $8.000$ | $< 0.01$ |
| **Q1: Hypergeometric** | Variance ($\text{Var}(X)$) | $\approx 3.879$ (as $n \to 10^4$) | $3.8788$ | $< 0.02$ |
| **Q2: Binomial Approx** | Without Correction ($X \le 430$) | $0.1018139$ | $0.1074638$ (Exact) | $5.65 \times 10^{-3}$ |
| **Q2: Binomial Approx** | With Correction ($X \le 430.5$) | **$0.1075799$** | $0.1074638$ (Exact) | **$1.16 \times 10^{-4}$** |
| **Q3: Memorylessness** | $P(X \le 15 \mid X > 12)$ | **$0.1812429$** | **$0.1812692$** | **$2.63 \times 10^{-5}$** |
| **Q4: Exp Transform** | $Y = -2 \ln(U)$ Density | Matches $\text{Exp}(0.5)$ | $\frac{1}{2}e^{-y/2}$ | Exact visual fit |
| **Q4: Box-Muller** | $Z_1, Z_2$ Distribution | $\mathcal{N}(0, 1)$ Density | Standard Normal | Exact visual fit |

---

## 💻 Environment & Execution Guide

### Prerequisites

- **R** (version 4.0 or higher)
- **Jupyter Notebook / JupyterLab** with the **IRkernel** package installed

### Installing IRkernel in R

If running within Jupyter, configure the R kernel from within an R console:

```R
install.packages("IRkernel")
IRkernel::installspec(user = FALSE)
```

### Running the Notebooks

Launch Jupyter and open each notebook:

```bash
cd CA1
jupyter notebook Q1.ipynb
```

Alternatively, you can open and run any of the `.ipynb` files directly in **VS Code** (with the R and Jupyter extensions installed) or convert them to `.R` scripts for execution via RStudio.
