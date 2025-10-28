# Quantum Portfolio Optimization: A VQE & QAOA Approach

![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)
![Qiskit](https://img.shields.io/badge/Qiskit-0.45%2B-blueviolet.svg)
![Finance](https://img.shields.io/badge/Domain-Finance-brightgreen.svg)
![Algorithm](https://img.shields.io/badge/Algorithms-VQE%20%7C%20QAOA-orange.svg)

This project applies and evaluates quantum optimization algorithms—specifically the **Variational Quantum Eigensolver (VQE)** and **QAOA-Ansatz**—to solve a real-world financial problem: Markowitz portfolio optimization.

The goal is to find the optimal allocation of assets to maximize expected returns for a given level of risk. We compare the performance of these quantum algorithms against both an exact classical solver (`NumpyMinimumEigensolver`) and a classical sampling-based heuristic (`SamplingMinimumEigensolver`).

## 📊 The Problem: Mean-Variance Optimization

The core of this project is to solve the Markowitz mean-variance optimization problem. We aim to construct a portfolio of $n$ assets that minimizes the objective function:

$\min_{x} \{ q \cdot x^T \Sigma x - (1-q) \cdot \mu^T x \}$

Where:
* $x$ is the binary vector of asset allocations (1 if selected, 0 if not).
* $\mu$ is the vector of expected returns for each asset.
* $\Sigma$ is the covariance matrix, representing the risk.
* $q$ is the risk aversion parameter (set to `0.8` in this project, prioritizing risk minimization).

For this project, we analyzed **10 assets** (`AAPL`, `MSFT`, `GOOG`, `AMZN`, `TSLA`, `JPM`, `JNJ`, `V`, `PG`, `NVDA`) with a budget constraint to select exactly **5**.

## 🛠️ Methodology

The project follows these key steps:

1.  **Data Ingestion:** Historical stock data is downloaded using `yfinance`.
2.  **Problem Formulation:** We calculate the expected returns ($\mu$) and the covariance matrix ($\Sigma$) from the historical data.
3.  **Optimization Model:** A `QuadraticProgram` is defined in `qiskit-optimization` to represent the problem, including the budget constraint.
4.  **QUBO Conversion:** The problem is converted to an unconstrained QUBO (Quadratic Unconstrained Binary Optimization) problem, which can be mapped to an Ising Hamiltonian for the quantum solvers.
5.  **Solver Execution:** We solve the problem using four distinct methods on a quantum simulator:
    * **Classical Exact (`NumpyMinimumEigensolver`):** Provides the ground truth, optimal solution.
    * **Classical Heuristic (`SamplingMinimumEigensolver`):** A classical sampling-based approach.
    * **Quantum (VQE):** The Variational Quantum Eigensolver using a `RealAmplitudes` ansatz and an `SPSA` optimizer.
    * **Quantum (QAOA-Ansatz):** VQE using a `QAOAAnsatz` and an `SPSA` optimizer.
6.  **Backtesting:** The total return of each selected portfolio is calculated using a separate test data period to evaluate real-world performance.
# VQE (from Cell #13): 9.48% Total Returns
# QAOA (from Cell #16): 10.83% Total Returns
# Classical (from Cell #17): 8.27% Total Returns
# Naive (from Cell #17): 5.45% Total Returns


## 📦 Requirements

To run this project, you will need the libraries listed in `requirements.txt`.

```bash
pip install -r requirements.txt
