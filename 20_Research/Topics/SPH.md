---
aliases: [Smoothed Particle Hydrodynamics, smoothed particle hydrodynamics]
tags: [topic]
---

# SPH

Smoothed Particle Hydrodynamics — a Lagrangian particle-based method for fluid simulation. Particles carry physical quantities (density, velocity, pressure) and interact through radially symmetric kernel functions. Variants include WCSPH (weakly compressible), PCISPH (predictive-corrective), DFSPH (divergence-free), and IISPH (implicit incompressible).

## Key Concepts
- Kernel function and support radius
- Density estimation and kernel correction (e.g., Shepard correction)
- Pressure solvers: equation of state (WCSPH), pressure Poisson (IISPH/DFSPH)
- Viscosity models: artificial viscosity, XSPH
- Boundary handling: ghost particles, penalty forces
- Neighborhood search: spatial hashing, compact hashing

## Papers
- [[20_Research/Papers/Simulation/A PML-based Nonreflective Boundary for Free Surface Fluid Animation|A PML-based Nonreflective Boundary for Free Surface Fluid Animation]]
- [[20_Research/Papers/Simulation/Consistent Shepard Interpolation for SPH-based Fluid Animation|Consistent Shepard Interpolation for SPH-based Fluid Animation]]
- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]]
