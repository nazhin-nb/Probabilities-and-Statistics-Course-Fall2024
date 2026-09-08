# Engineering Probability & Statistics — Coursework Portfolio (Fall 2024)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![R](https://img.shields.io/badge/Language-R-276DC3.svg)](https://www.r-project.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.20%2B-orange.svg)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-1.8%2B-navy.svg)](https://scipy.org/)
[![SimPy](https://img.shields.io/badge/SimPy-4.x-brightgreen.svg)](https://simpy.readthedocs.io/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-green.svg)](https://scikit-learn.org/)
[![Course](https://img.shields.io/badge/Course-Engineering_Probability_%26_Statistics_Fall_2024-purple.svg)](#)

> **Author:** Nazhin Nikkhahbahrami  
> **Student ID:** 810102530  
> **Department:** Electrical & Computer Engineering, University of Tehran  
> **Course:** Engineering Probability & Statistics — Fall 2024  
> **Instructors:** Dr. Tavassolipour, Dr. Vahabi

---

## 📌 Repository Overview

This repository contains the complete collection of practical computer assignments (**CA0** through **CA3**) for the **Engineering Probability and Statistics** course at the **University of Tehran (Fall 2024)**. 

The coursework bridges core probabilistic theorems, statistical modeling, stochastic simulations, and data-driven inference using both **Python** and **R**. Each assignment folder contains fully executed notebooks, empirical datasets, and a **dedicated README** with thorough mathematical derivations and quantitative results.

---

## 📚 Curriculum & Assignments Summary

| Assignment | Core Topics & Focus Areas | Language / Stack | Status | Documentation |
| :--- | :--- | :--- | :---: | :---: |
| [**CA0**](CA0/) | **NumPy Fundamentals, Bayesian Decision Theory & Spam Detection**<br>Vectorization, array broadcasting, Condorcet's Jury Theorem simulations, Bayesian posterior voting accuracy, and Bag-of-Words text classification using Multinomial Naive Bayes ($98.78\%$ accuracy). | Python<br>*(NumPy, scikit-learn, Seaborn)* | ✅ Completed | [CA0 README](CA0/README.md) |
| [**CA1**](CA1/) | **Statistical Distributions, Limit Theorems & Transformations in R**<br>Hypergeometric election audit modeling, asymptotic convergence to Binomial, De Moivre–Laplace Normal approximation with continuity correction ($48\times$ error reduction), exponential queue memorylessness, and Box-Muller transformation. | R<br>*(IRkernel, Base R)* | ✅ Completed | [CA1 README](CA1/README.md) |
| [**CA2**](CA2/) | **Bayesian Estimation, Joint Distributions & Correlation / Causality**<br>SimPy $M/M/1$ queue simulation, KDE bandwidth tuning, conjugate Beta-Binomial updating over 1,000 coin flips, from-scratch Pearson correlation engine, electricity time-series seasonality, and spurious correlation vs. causality analysis. | Python<br>*(SimPy, SciPy, Pandas, Seaborn)* | ✅ Completed | [CA2 README](CA2/README.md) |
| [**CA3**](CA3/) | **Central Limit Theorem, MSE Optimization & De Moivre–Laplace**<br>CLT convergence across Poisson, Exponential, and Geometric populations, breakdown of CLT in non-i.i.d. mixtures, closed-form simple & matrix linear regression via Normal Equation, and Riemann scaling for Binomial-to-Normal approximations. | Python<br>*(NumPy, SciPy, scikit-learn, Matplotlib)* | ✅ Completed | [CA3 README](CA3/README.md) |

---

## 📁 Repository Structure

```text
Probabilities-and-Statistics-Course-Fall2024/
├── CA0/                                # Computer Assignment 0
│   ├── numpy_question/                 # NumPy basics and vectorization exercises
│   ├── Q2.ipynb                        # Majority voting & Bayesian decision theory
│   ├── Q3.ipynb                        # Spam email classification with Naive Bayes
│   ├── emails.csv                      # Email spam/ham dataset
│   ├── EPS_CA0.pdf                     # Assignment description
│   └── README.md                       # CA0 documentation
├── CA1/                                # Computer Assignment 1 (Implemented in R)
│   ├── Q1.ipynb                        # Hypergeometric & Binomial convergence
│   ├── Q2.ipynb                        # Normal approximation & continuity correction
│   ├── Q3.ipynb                        # Exponential queue & memoryless property
│   ├── Q4.ipynb                        # Box-Muller standard normal transformation
│   ├── CA1-Nikkhahbahrami-810102530.pdf# Comprehensive coursework technical report
│   ├── EPS_CA1.pdf                     # Assignment description
│   └── README.md                       # CA1 documentation
├── CA2/                                # Computer Assignment 2
│   ├── data/                           # Datasets (coin flips, energy load, TV & life expectancy)
│   ├── coin.ipynb                      # Conjugate Beta-Binomial Bayesian parameter estimation
│   ├── energy.ipynb                    # Custom correlation engine & seasonal load analysis
│   ├── pmf.ipynb                       # SimPy queue simulation & joint distributions
│   ├── correlation_matrix.csv          # Exported correlation matrix
│   ├── EPS_CA2.pdf                     # Assignment description
│   └── README.md                       # CA2 documentation
├── CA3/                                # Computer Assignment 3
│   ├── Question1.ipynb                 # Central Limit Theorem & mixture breakdown
│   ├── Question2-MSE.ipynb             # Linear regression & dataset center via MSE
│   ├── MSE.ipynb                       # Assignment template notebook
│   ├── Question3.ipynb                 # De Moivre-Laplace CLT & Riemann scaling
│   ├── EPS_CA3.pdf                     # Assignment description
│   └── README.md                       # CA3 documentation
├── LICENSE                             # MIT License
└── README.md                           # Master repository documentation
```

---

## 🔬 Core Methodological Themes

1. **Analytical & Numerical Probability:**
   - Exact discrete/continuous CDF and PMF evaluations (Binomial, Hypergeometric, Poisson, Geometric, Exponential, Gaussian, Beta).
   - Asymptotic limit theorems: Law of Large Numbers (LLN), De Moivre–Laplace Theorem, and the Central Limit Theorem (CLT) with boundary conditions and mixture breakdowns.

2. **Bayesian Decision Theory & Updating:**
   - Prior-to-posterior transition using Bayes' theorem in collective voting models.
   - Sequential conjugate Beta-Binomial updating with asymptotic convergence (Bernstein–von Mises theorem).

3. **Stochastic Simulation & Transformation:**
   - Discrete-event queueing simulations using SimPy ($M/M/1$).
   - Random variate generation via Inverse Transform Sampling and Box-Muller polar transformations.

4. **Statistical Modeling & Causal Inference:**
   - Custom covariance and correlation estimation from scratch.
   - Closed-form Mean Squared Error (MSE) minimization, Ordinary Least Squares (OLS), and multivariate Normal Equation $\theta = (X^T X)^{-1} X^T y$.
   - Differentiating mathematical correlation from causal mechanisms in socio-demographic indicators.

---

## 💻 Environment Setup & Prerequisites

### 1. Python Environment

Ensure **Python 3.8+** is installed, then install all project dependencies:

```bash
pip install numpy scipy pandas scikit-learn matplotlib seaborn simpy
```

### 2. R Environment (for CA1)

Ensure **R (version 4.0+)** is installed. To execute the R notebooks in Jupyter, install the `IRkernel` package inside an R console:

```R
install.packages("IRkernel")
IRkernel::installspec(user = FALSE)
```

### 3. Launching Notebooks

To browse and run any assignment, launch Jupyter from the repository root:

```bash
jupyter notebook
```

---

## 📜 License

This project is licensed under the [MIT License](LICENSE) — feel free to use and reference the code with appropriate attribution.