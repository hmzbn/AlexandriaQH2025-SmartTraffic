
# Smart Traffic Optimization — Emergency Patient Transportation

**Objective:** Minimize total distance to transport 5 patients to *Central Hospital* with one ambulance, up to 3 stops per trip.

## Problem & Data
- Depot: Hospital at (29.995127, 31.684628)
- Patients: 5 GPS points (see solution).
- Constraints: 1 vehicle, ≤3 pickups per trip, multiple trips allowed.

## Classical Baseline
- Compute **haversine** distances between all points.
- Enumerate all partitions of patients into groups of size ≤3.
- For each group, evaluate all visit permutations to get its **shortest tour** (H -> ... -> H).
- Sum tour costs across groups and choose the **minimum**.
- For 5 patients this is tractable and gives the **global optimum**.
- Result is provided in *01_classical_routing.ipynb*.

## QUBO (Quantum) Modeling
We encode a small multi-trip VRP as a **QUBO** with binary variables `x[i,t,p]` meaning *patient i is served at position p in trip t*.
- **Constraints** are turned into penalties:
  1. Each patient appears **exactly once** (quadratic penalty on (Σx − 1)^2).
  2. Each position has **≤1** patient (pairwise penalties on x_a x_b).
- **Objective** sums GPS distances for: depot→first, between consecutive positions, last→depot.
- With 5 patients, K=3, T=2 trips → 30 binary variables.

We solve with **QAOA** (simulator).

## Comparative Notes
- **Classical**: exact for this scale; scales factorially with patients per trip and combinatorially with grouping.
- **Quantum (QAOA/QUBO)**: offers a unified formulation; practical performance depends on problem size, penalty tuning, and filtered out the invalid bitstrings.

## Tools & Platforms
- Python 3, NumPy, Jupyter
- Qiskit 

*See the notebooks (solution.ipynb and quantum_solution.ipynb) for full details and executable code.*
