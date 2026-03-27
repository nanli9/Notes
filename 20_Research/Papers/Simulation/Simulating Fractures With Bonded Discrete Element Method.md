---
paper_id: "10.1109/TVCG.2021.3106738"
title: "Simulating Fractures With Bonded Discrete Element Method"
authors: ["Jia-Ming Lu", "Chen-Feng Li", "Geng-Chen Cao", "Shi-Min Hu"]
year: 2022
venue: "IEEE Transactions on Visualization and Computer Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, fracture, DEM, particle-based, bonded-elements, destruction]
quality_score: 16.9
related_papers: ["10.1145/311535.311550", "10.1145/3306346.3322949", "10.1145/2766896"]
created: "2026-03-20"
status: "analyzed"
---

# Simulating Fractures With Bonded Discrete Element Method

**DOI:** [10.1109/TVCG.2021.3106738](https://doi.org/10.1109/TVCG.2021.3106738) · **Tag:** `10.1109/TVCG.2021.3106738`

## Plain-English Summary

Introduces the **Bonded Discrete Element Method (BDEM)** to computer graphics for [[fracture simulation]]. Instead of representing objects as continuous meshes (like [[FEM]]) or particle-grid hybrids (like [[MPM]]), BDEM models solids as collections of spherical particles connected by deformable bonds. When bond stress exceeds material strength, the bond breaks — fracture emerges naturally from individual bond failures without any mesh splitting, remeshing, or explicit crack tracking. The approach is adapted from engineering DEM research with several modifications tailored for animation design.

## Actual Novelty

1. **First BDEM framework for graphics:** adapts the Bonded DEM (well-studied in geomechanics/engineering) for fracture animation, introducing modifications that make it practical for visual effects.
2. **Four-mode bond model:** each bond transmits stretch, shear, bend, and twist — capturing richer deformation than simple spring-breaking approaches while remaining straightforward to implement.
3. **Superior scale consistency:** unlike FEM fracture which produces qualitatively different crack patterns at different resolutions, BDEM maintains consistent fracture behavior across scales — a significant practical advantage for production use.

## Method

### Discrete Element Setup
- Object filled with spherical discrete elements (particles with position + orientation)
- Neighboring elements connected by **bonds** — each bond is a mechanical link transmitting forces and torques
- Elements carry both translational and rotational degrees of freedom

### Bond Deformation (4 Modes)
- **Stretch:** $V_{\text{stretch}} = \frac{1}{2} k_n (\|p_i - p_j\| - l_0)^2$ — normal spring along bond axis
- **Shear:** $V_{\text{shear}} = \frac{1}{2} k_t \theta^2$ — tangential displacement perpendicular to bond
- **Bend & Twist:** $V_{\text{bt}} = \frac{1}{2} \mathbf{t}^T K \mathbf{t}$ — separate stiffnesses for bending (second moment $I$) and twisting (polar moment $J$)
- Stiffness parameters derived from Young's modulus, cross-sectional area, and bond geometry

### Fracture Criterion
- Tensile stress: $\sigma = \frac{\|F_n\|}{S} + \frac{\|M_b\| r_0}{I}$
- Shear stress: $\tau = \frac{\|F_s\|}{S} + \frac{\|M_t\| r_0}{J}$
- **Bond breaks when** $\sigma > \sigma_c$ or $\tau > \tau_c$ (material tensile/shear strength)
- Multiple fractures handled naturally — each bond fails independently

### Simulation Loop
1. Compute bond deformations from particle positions and orientations
2. Derive forces and torques from bond energies
3. Check fracture criterion per bond — remove broken bonds
4. Integrate positions and orientations (explicit time stepping)
5. Handle inter-fragment contact via standard DEM collision

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared against FEM fracture and MPM fracture for scale consistency |
| Metrics | Scale consistency analysis, visual fidelity, varied material examples |
| Runtime | Computationally expensive — requires small time steps (explicit integration) |
| Failure cases | High computational cost is the primary bottleneck; explicit integration limits step size |

## Likely Weaknesses

- **Computational cost:** explicit time integration demands very small time steps for stability — significantly slower than implicit FEM or MPM approaches
- **Particle packing artifacts:** fracture surfaces inherit the discrete element distribution, which can look granular rather than smooth
- **No implicit integration:** limits scalability to large scenes (addressed in the 2025 follow-up: Implicit BDEM with Manifold Optimization)
- **Bond parameter calibration:** mapping macroscopic material properties (Young's modulus, Poisson's ratio) to microscopic bond parameters is non-trivial
- **Resolution-dependent detail:** finer particle packing needed for small crack features, increasing cost quadratically/cubically

## Verdict

A compelling alternative paradigm for fracture in graphics. Where FEM fracture fights mesh topology changes and MPM fracture produces diffuse cracks, BDEM sidesteps both issues entirely — bonds simply break, fragments separate, done. The scale consistency advantage over FEM is a real practical win. The main barrier is computational cost from explicit integration, but the follow-up implicit BDEM work (Lu et al., TOG 2025) achieves 2–10× speedups. Recommended reading for anyone exploring fracture methods beyond the FEM/MPM mainstream — especially if you want a method where fracture is the natural behavior rather than an added complication.

## Related Papers

- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|O'Brien & Hodgins 1999]] — foundational FEM fracture; BDEM shows better scale consistency
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]] — MPM-based fracture alternative; diffuse cracks vs. BDEM's sharp bond-breaking
- [[20_Research/Papers/Simulation/High-Resolution Brittle Fracture Simulation with Boundary Elements|Hahn & Wojtan 2015]] — BEM fracture; high-resolution crack surfaces but mesh-based
- [[20_Research/Papers/Simulation/Fast Approximations for Boundary Element Based Brittle Fracture Simulation|Fast BEM Fracture]] — Hahn & Wojtan 2016; linear-time BEM cracks
