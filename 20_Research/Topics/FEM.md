---
title: "FEM — Finite Element Method"
type: topic
tags: [topic, Simulation, numerical-methods, deformable]
---

# FEM — Finite Element Method

Numerical method for solving PDEs by discretizing the domain into elements (triangles, tetrahedra). The standard tool for high-fidelity [[deformable body]] simulation in graphics.

**In graphics simulation:**
- Tetrahedral FEM for volumetric soft bodies
- Triangle / quadrilateral shell elements for [[cloth simulation]] and [[thin shell]]
- Constitutive models: Neo-Hookean, co-rotated, StVK

**Time integration:**
- Implicit (stable, expensive) — Newton / PCG solves per step
- Explicit (fast, conditionally stable — step size limited by stiffness)
- IMEX — hybrid

**Solvers:**
- [[PCG]] with preconditioners ([[GPU simulation]] variants)
- Direct sparse solvers (Cholesky)
- Iterative: [[IPC]] uses Newton + line search

## Course Notes

### Continuum Mechanics Foundation
- Deformation: $x = X + u$, deformation gradient $F = \frac{\partial x}{\partial X} = I + \frac{\partial u}{\partial X}$ (F is different for each X)
- Energy density function: $d\mathcal{E} = \psi(F) \cdot dV$, total energy $\mathcal{E} = \int \psi(F) \, dV$

### Material Parameters
- **Young's modulus:** $E = \frac{\sigma}{\varepsilon} = \frac{F \cdot l}{S \cdot \Delta L}$
- **Poisson's ratio:** $\nu$, range $-\frac{1}{2} \leq \nu \leq \frac{1}{2}$
- **Lamé parameters:** $\lambda = \frac{\nu \cdot E}{(1+\nu)(1-2\nu)}$, $\mu = \frac{E}{2(1+\nu)}$

### Strain & Constitutive Model
- **Green-Lagrange strain:** $\mathbf{E} = \frac{1}{2}(F^T F - I)$
- **St. Venant-Kirchhoff (StVK):** $\psi(F) = \frac{1}{2}(\ln(\mathbf{E}))^2 + \mu \, \mathbf{E} : \mathbf{E}$

### FEM Discretization
- FEM assumption: $u(X) = \sum_i \varphi_i(X) \cdot u_i$ (shape functions)
- Discretized state: $\hat{u} = [u_0, u_1, \ldots, u_T]^T \in \mathbb{R}^{2d}$ (d = dim per node)
- Energy becomes: $\mathcal{E} = \int \psi(I + \frac{\partial}{\partial X} \varphi_i(X) u_i) \, dV = \mathcal{E}(\hat{u})$
- Internal forces: $f_{\text{int}} = -\frac{\partial \mathcal{E}}{\partial \hat{u}}$
- **Quadrature:** $\int f \, dV \approx \sum_i w_i f(x_i)$

### Corotational Linear FEM
- Equation of motion: $M\ddot{u} + D\dot{u} + f_{\text{int}}(u) = f_{\text{ext}}$, where $f_{\text{int}}(u) = \frac{\partial \mathcal{E}}{\partial u}$
- Stiffness: $M\ddot{u} + D\dot{u} + K \cdot u = F$
- **Consistent mass matrix:** $M_{ij} = \int \varphi_i(X) \varphi_j(X) \, dV$
- Deformation gradient $F \in \mathbb{R}^{3\times3}$
- **Polar decomposition:** $F = R \cdot S$ (3×3 rotation × 3×3 symmetric)
  - Find R: $\min_{R \in SO(3)} \|F - R\|_F^2$
  - Extracts rotation per element → removes rigid rotation before computing linear strain → re-applies after

### Trace
- $\text{tr}(A) = A_{11} + A_{22} + A_{33}$

## Papers
- [[Progressive Dynamics for Cloth and Shell Animation]] — FEM shells with IPC contact
- [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]] — GPU FEM solver
- [[Deformable Object Animation Using Reduced Optimal Control]] — reduced FEM
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]]
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|Graphical Modeling and Animation of Brittle Fracture]]
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Ductile Fracture|Graphical Modeling and Animation of Ductile Fracture]]
- [[20_Research/Papers/Simulation/Fast Simulation of Skeleton-Driven Deformable Body Characters|Fast Simulation of Skeleton-Driven Deformable Body Characters]]
- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]]
- [[20_Research/Papers/Simulation/Simulating Fractures With Bonded Discrete Element Method|Simulating Fractures With Bonded Discrete Element Method]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains|A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains]]
- [[20_Research/Papers/Simulation/Characterizing Long-Range Dependencies in Knee Joint Contact Mechanics|Characterizing Long-Range Dependencies in Knee Joint Contact Mechanics]]
