# Engineering Probability & Statistics — Computer Assignment 0 (CA0)

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-1.20%2B-orange.svg)](https://numpy.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0%2B-green.svg)](https://scikit-learn.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.5%2B-red.svg)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12%2B-lightblue.svg)](https://seaborn.pydata.org/)
[![Course](https://img.shields.io/badge/Course-Probabilities_%26_Statistics_Fall_2024-purple.svg)](#)

> **Author:** Nazhin Nikkhahbahrami  
> **Student ID:** 810102530  
> **Department:** Electrical & Computer Engineering, University of Tehran  
> **Course:** Engineering Probability & Statistics (Fall 2024)

---

## 📌 Overview

This project comprises **Computer Assignment 0 (CA0)** for the Engineering Probability and Statistics course. The assignment focuses on establishing computational and theoretical foundations in probability, numerical computing, and machine learning:
1. **NumPy Fundamentals & Vectorization:** Array manipulation, multidimensional indexing, slicing, broadcasting, and batched matrix operations.
2. **Majority Voting & Bayesian Decision Theory (Condorcet's Jury Theorem):** Probabilistic modeling of collective decision-making, posterior probability computation using Bayes' theorem, and extensive Monte Carlo simulations.
3. **Spam Classification with Naive Bayes:** Data preprocessing, Bag-of-Words (BoW) text representation, and probabilistic spam email detection using Multinomial Naive Bayes.

---

## 📁 Repository Structure

```text
CA0/
├── numpy_question/
│   ├── numpy_basic.ipynb     # Interactive notebook validating NumPy exercises
│   └── numpy_basic.py        # Python module containing core NumPy implementations
├── Q2.ipynb                  # Majority voting simulation & Bayesian analysis
├── Q3.ipynb                  # Spam classification pipeline with Naive Bayes
├── emails.csv                # Dataset containing labeled spam/ham emails
├── EPS_CA0.pdf               # Assignment problem description
└── README.md                 # Project documentation
```

---

## 🔬 Completed Work Breakdown

### Question 1: NumPy Foundations & Vectorized Computing (`numpy_question/`)

Implemented in [`numpy_basic.py`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA0/numpy_question/numpy_basic.py) and validated via [`numpy_basic.ipynb`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA0/numpy_question/numpy_basic.ipynb):

- **Array Basics & Creation:**
  - `create_sample_array`: Construction of custom 2D rank-2 arrays.
  - `mutate_array`: In-place mutation at designated indices.
  - `count_array_elements`: Dynamic shape inspection and total element counting.
  - `create_array_of_pi`: Generating constant floating-point arrays initialized with $\pi$.
  - `multiples_of_ten`: Sequence generation with explicit 64-bit integer datatypes.
- **Array Indexing & Slicing:**
  - `slice_indexing_practice` & `slice_assignment_practice`: Slicing subarrays, mutating rectangular regions, and understanding array views vs. copies.
  - `shuffle_cols`, `reverse_rows`, and `take_one_elem_per_col`: Integer array indexing for arbitrary dimension permutations and coordinate extractions.
- **Boolean Masking & Encodings:**
  - `count_negative_entries`: Conditional filtering and boolean array indexing.
  - `make_one_hot`: Transforming categorical class labels into one-hot indicator matrices.
- **Reshaping & Axes Permutation:**
  - `reshape_practice`: Multidimensional tensor flattening, dimension resizing, and axis swapping (`np.transpose`).
- **Reductions & Vectorized Linear Algebra:**
  - `zero_row_min`: Row-wise minimum extraction and zeroing out minimal values using broadcasting.
  - `batched_matrix_multiply`: Tensor batch multiplication ($B \times N \times M$ with $B \times M \times P$) implemented via looping and vectorization.
  - `normalize_columns`: Feature scaling and column-wise zero-mean/unit-variance standardization using broadcasting rules.

---

### Question 2: Majority Voting Dynamics & Bayesian Inference (`Q2.ipynb`)

Implemented and analyzed in [`Q2.ipynb`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA0/Q2.ipynb):

- **Part A — Bayesian Posterior Accuracy:**
  Formulated posterior belief using Bayes' Rule under equal prior probabilities ($P(\text{Correct}) = P(\text{Incorrect}) = 0.5$):
  $$P(\text{Correct} \mid k \text{ votes}) = \frac{0.5 \cdot p^k (1-p)^{N-k}}{0.5 \cdot p^k (1-p)^{N-k} + 0.5 \cdot (1-p)^k p^{N-k}}$$
  Evaluated across 5 predefined test scenarios:
  | Scenario | Voter Accuracy ($p$) | Votes for "1" | Votes for "0" | Majority Posterior Accuracy |
  | :---: | :---: | :---: | :---: | :---: |
  | **1** | 0.70 | 8 | 4 | **96.74%** |
  | **2** | 0.70 | 10 | 2 | **99.89%** |
  | **3** | 0.30 | 8 | 4 | **3.26%** |
  | **4** | 0.50 | 9 | 3 | **50.00%** |
  | **5** | 0.50 | 5 | 7 | **50.00%** |

- **Part B — Monte Carlo Voting Simulation:**
  - Developed `simulateVoting(numOfVoters, p)` with 10,000 Monte Carlo trials per setting.
  - Simulating group decision outcomes for $N = 12$ voters across individual accuracies $p \in [0.1, 1.0]$.
  - Plotted collective accuracy curve, illustrating the **Condorcet Jury Theorem**: when $p > 0.5$, group accuracy strictly exceeds individual competence; when $p < 0.5$, group voting degrades performance.

- **Part C — Optimal Competence Search:**
  - Algorithmically searched for the individual voter accuracy threshold that achieves 100% empirical collective accuracy ($1.0$ in 10,000 runs) for $N = 12$ voters.
  - Result: **Optimal Accuracy threshold $p^* = 0.93$**.

- **Part D — 2D Parameter Phase Space Heatmap:**
  - Comprehensive grid evaluation across group sizes $N \in [1, 50]$ and voter competencies $p \in [0.1, 1.0]$.
  - Rendered a high-resolution Seaborn heatmap (`coolwarm` palette), demonstrating the rapid convergence toward deterministic correctness as $N$ grows when $p > 0.5$.

---

### Question 3: Spam Classification via Naive Bayes (`Q3.ipynb`)

Implemented in [`Q3.ipynb`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA0/Q3.ipynb) using [`emails.csv`](file:///d:/Antigravity/Probabilities-and-Statistics-Course-Fall2024/CA0/emails.csv):

- **Part A — Data Preprocessing Pipeline:**
  - Ingestion of raw email records from CSV.
  - `cleanText` implementation: lowercasing, regex-based digit removal (`re.sub(r'\d+', '', text)`), and punctuation removal using character translation tables (`string.punctuation`).
- **Part B — Train / Test Data Splitting:**
  - Stratified split of text and label corpora into **80% Training** and **20% Testing** partitions using scikit-learn's `train_test_split` (`random_state=42`).
- **Part C — Feature Extraction & Classification:**
  - Extracted word count frequency features using `CountVectorizer` (Bag-of-Words).
  - Trained a `MultinomialNB` classifier on the training set BoW representations.
  - Evaluated performance on unseen test data:
    - **Test Accuracy:** **98.78%** (`Accuracy = 0.9878`)
    - Computed confusion matrix for classification breakdown.

*(Note: Incomplete sections from the original assignment prompt, such as Part D scratch likelihood computation and theoretical open questions on zero-frequency smoothing and underflow log-transforms, are intentionally omitted as they were not implemented.)*

---

## 📊 Summary of Key Results

| Component | Metric / Task | Result / Observation |
| :--- | :--- | :--- |
| **NumPy Basics** | Unit Tests (`numpy_basic.ipynb`) | 100% Passed (All tests evaluated to `True`) |
| **Q2 Part A** | Bayes Posterior (Scenario 2: $p=0.7$, 10 vs 2) | **99.89%** posterior certainty |
| **Q2 Part C** | Optimal Voter Accuracy ($N=12$) | $p^* = \mathbf{0.93}$ for empirical certainty |
| **Q2 Part D** | 2D Simulation Grid | Phase transition observed at $p = 0.5$ |
| **Q3 Part C** | Naive Bayes Spam Detection Accuracy | **98.78%** on test set |

---

## 💻 Requirements & Setup

### Prerequisites

- Python 3.8+
- Jupyter Notebook / JupyterLab

### Dependencies

Install the required packages using `pip`:

```bash
pip install numpy scikit-learn matplotlib seaborn
```

### Running the Project

1. **NumPy Fundamentals:**
   ```bash
   cd CA0/numpy_question
   jupyter notebook numpy_basic.ipynb
   ```
2. **Majority Voting & Bayesian Simulations:**
   ```bash
   cd CA0
   jupyter notebook Q2.ipynb
   ```
3. **Spam Email Classifier:**
   ```bash
   cd CA0
   jupyter notebook Q3.ipynb
   ```
