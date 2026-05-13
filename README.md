# Topic 2.A — Classical Shadows with Randomized Pauli Measurements

Implementation of the classical shadows protocol using randomized Pauli measurements to estimate second Renyi entropies of the time-evolved Neel state under the XY-Hamiltonian.

**Course:** Applied Quantum Algorithms — Mini Project  
**Reference:** [arXiv:2203.11374](https://arxiv.org/abs/2203.11374), Section II.D

## Problem

Given an N-qubit Neel state evolving under the XY-Hamiltonian

$$H_{XY} = \sum_{i=0}^{N-2} \frac{X_i X_{i+1} + Y_i Y_{i+1}}{2}$$

estimate the second Renyi entropy $S_2(\rho_A) = -\log_2 \mathrm{Tr}(\rho_A^2)$ of subsystems using classical shadows, and compare against exact state-vector simulation.

## What's Implemented

| Component | Description |
|-----------|-------------|
| **Neel state preparation** | Circuit preparing $\|1010101010\rangle$ on 10 qubits |
| **Trotter time evolution** | First-order Trotterization of $e^{-iH_{XY}t}$ using `qml.IsingXX` / `qml.IsingYY` |
| **Exact Renyi entropy** | State-vector partial trace to compute $S_2(\rho_A)$ for all subsystem sizes |
| **Random Pauli measurements** | Simulation of M random Pauli basis choices with K shots each |
| **Shadow Renyi estimator** | Cross-correlator: estimate \text{Tr}(\rho_A^2) from pairwise outcome matches across    shots (Eq. 3 in paper) |
| **Exact vs Shadow comparison** | Side-by-side plots for $t \in [0, 1]$ (analogous to Fig. 3b in the paper) |
| **M, K dependence (8+)** | Convergence analysis with error bars and error heatmap |

## Results

- **Exact entropies** show a characteristic dome shape that grows with time, peaking near the chain center.
- **Classical shadows** (M=500, K=50) recover exact values with typical errors < 0.1 for most subsystem sizes.
- **Convergence:** Increasing M reduces variance from basis sampling; increasing K improves the cross-correlator. Both contribute to the total measurement budget.

## Requirements

```
numpy
pennylane
matplotlib
```

Install with:

```bash
pip install numpy pennylane matplotlib
```

## Usage

Open and run `main.ipynb` in Jupyter:

```bash
jupyter notebook main.ipynb
```

Run cells sequentially. Sections 1-5 complete in under a minute. Section 6 (M, K parameter sweep) takes a few minutes.

## Project Structure

```
.
├── README.md
└── main.ipynb   # Full implementation notebook
```

## Key Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `N_QUBITS` | 10 | Number of qubits in the chain |
| `N_TROTTER_STEPS` | 20 | Trotter steps for time evolution |
| `M` | 500 | Random measurement settings |
| `K` | 50 | Shots per measurement setting |
