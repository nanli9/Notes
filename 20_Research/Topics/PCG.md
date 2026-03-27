---
title: "PCG — Preconditioned Conjugate Gradient"
type: topic
tags: [topic, Simulation, numerical-methods, linear-solver]
---

# PCG — Preconditioned Conjugate Gradient

Iterative solver for symmetric positive-definite (SPD) linear systems `Ax = b`. The standard solver for implicit [[FEM]] time integration in physics simulation.

**Preconditioning:**
Applying a preconditioner `M⁻¹` transforms the system to one with better-conditioned eigenvalue distribution, reducing iteration count.

Common preconditioners for simulation:
- Diagonal (Jacobi) — trivial, weak
- ILU(0), ILUT — moderate quality, hard to parallelize
- Multigrid / multilevel — best quality, parallelizable
- MAS (Multilevel Additive Schwarz) — [[GPU simulation]]-friendly, see [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]]

**GPU considerations:**
- Conflict-free matrix-vector products needed
- Non-overlapping domain decomposition enables parallel preconditioning

## Papers
- [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]] — 4× GPU PCG speedup
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
