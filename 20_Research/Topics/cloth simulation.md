---
title: "Cloth Simulation"
type: topic
tags: [topic, Simulation, cloth, deformable]
---

# Cloth Simulation

Physics-based simulation of fabric, garments, and thin flexible sheets. Spans offline VFX-quality simulation to real-time game approximations.

**Core representations:**
- Triangle meshes (most common)
- Yarn-level / knit models (high fidelity, expensive)

**Key simulation methods:**
- Mass-spring (fast, conditionally stable)
- [[FEM]] on shells (more accurate material models)
- [[PBD]] / XPBD — real-time, unconditionally stable
- [[IPC]]-based — high-fidelity, intersection-free

**Key challenges:**
- [[contact]] and self-collision (hardest problem)
- Wrinkling and fine detail at interactive rates
- Friction (anisotropic, measurement-based)
- Coarse-to-fine upsampling for game budgets

## Course Notes

### Cloth Parametrization (Baraff-Witkin 1998)
- Reference: Baraff & Witkin, SIGGRAPH 1998 + stencil buffer
- Cloth surface: $(u,v) \mapsto w \in \mathbb{R}^3$
- Deformation gradients: $W_u = \frac{\partial w}{\partial u} \in \mathbb{R}^3$, $W_v = \frac{\partial w}{\partial v} \in \mathbb{R}^3$

### Energy Model
- **Stretch energy:** $C(x) = \begin{bmatrix} \|W_u\| - 1 \\ \|W_v\| - 1 \end{bmatrix}$, $E = \frac{k}{2} \langle C(x), C(x) \rangle = \frac{k}{2} \|C(x)\|^2$
  - Scaled by triangle area $a$
- **Shear energy:** $C_{\text{shear}} = a \cdot \langle W_u, W_v \rangle$
- **Bending energy:** $C_{\text{bend}}(x) \cdot \theta$ (dihedral angle between adjacent triangles)
- **Total:** $E_{\text{total}} = E_{\text{stretch}} + E_{\text{shear}} + E_{\text{bend}}$

### Forces & Integration
- Force: $f = -\frac{\partial E}{\partial x} = -k \frac{\partial C}{\partial x} \cdot C$
- Hessian: $H = \frac{\partial^2 E}{\partial x^2}$
- **Implicit backward Euler:** $\dot{y} = G(y)$, $y \in \mathbb{R}^k$, $G: \mathbb{R}^k \to \mathbb{R}^k$
  - $y_n = y_{n+1} - h \cdot G(y_{n+1})$
  - Linearize: $G(y_{n+1}) \approx G(y_n) + \frac{\partial G}{\partial y}\big|_{y=y_n} (y_{n+1} - y_n)$
  - System: $(I - h \frac{\partial G}{\partial y}\big|_{y=y_n}) \cdot \Delta y_n = h \cdot G(y_n)$ — a $k \times k$ matrix solve

### Equations of Motion (State-Space Form)
- $M\ddot{u} + D\dot{u} + f_{\text{int}}(u) = f_{\text{ext}}$
- State: $y = \begin{bmatrix} x \\ \dot{x} \end{bmatrix}$, $\dot{y} = \begin{bmatrix} \dot{x} \\ F(y_1, y_2) \end{bmatrix}$ where $\ddot{u} = M^{-1}(f_{\text{ext}} - D\dot{u} - f_{\text{int}}(u))$
- Jacobian $\frac{\partial G}{\partial y}$ assembled blockwise → implicit integration matrix

## Papers
- [[Progressive Dynamics for Cloth and Shell Animation]] — coarse-to-fine with IPC contact
- [[Robust Treatment of Collisions, Contact and Friction for Cloth Animation]] — foundational contact
- [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]] — GPU solver
- [[Super-Resolution Cloth Animation with Spatial and Temporal Coherence]] — upsampling
- [[20_Research/Papers/Simulation/Pattern-Based Cloth Registration and Sparse-View Animation|Pattern-Based Cloth Registration and Sparse-View Animation]]
- [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels]]
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|A Material Point Method for Thin Shells with Frictional Contact]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
- [[20_Research/Papers/Simulation/Efficient Simulation of Inextensible Cloth|Efficient Simulation of Inextensible Cloth]]
- [[20_Research/Papers/Simulation/I-cloth|I-cloth]]
