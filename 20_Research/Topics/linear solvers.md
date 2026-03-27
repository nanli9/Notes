---
aliases: [linear solvers, solving linear systems, Ax=b]
tags: [topic, numerical-methods]
---

# Linear Solvers

Methods for solving $Ax = b$. Central to physics simulation — every implicit time step involves at least one linear solve.

## Course Notes

### Decision Tree by Problem Size & Structure

**Small systems** (up to ~2000×2000):
- Is it symmetric?
  - **No** → Gaussian elimination ($A = PLU$)
  - **Yes** → Modified Cholesky ($A = PLDL^T$)
    - Check: SPD ⟺ all eigenvalues > 0
    - If $A$ is singular (zero eigenvalue) → special handling

**Large systems** (> 2000×2000):
- **Sparse?**
  - **Yes, Direct:** PARDISO, TAUCS, MUMPS, SPOOLES
  - **Yes, Iterative:**
    - SPD → [[PCG]] (Preconditioned Conjugate Gradient)
    - Symmetric (indefinite) → MINRES
    - General (non-symmetric) → GMRES
  - **No (dense)** → $O(n^3)$, rarely encountered in simulation

### Software
- **Solvers:** MATLAB, GNU Octave
- **Libraries:** BLAS, LAPACK, Intel MKL
- **C++ linear algebra:** Eigen (header-only)

## Papers
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
