---
title: "Thin Shell Simulation"
type: topic
tags: [topic, Simulation, cloth, shell, codimensional]
---

# Thin Shell Simulation

Simulation of objects whose thickness is negligible compared to their lateral extent — cloth, membranes, sheet metal, biological tissue. Mathematically modeled as 2D surfaces embedded in 3D space (codimensional objects).

**Shell theories:**
- Kirchhoff-Love — no transverse shear, thin plate bending
- Reissner-Mindlin — includes transverse shear, thicker shells

**In graphics:**
- Triangle meshes with bending and stretching energy terms
- Discrete shells (Grinspun et al. 2003) — dihedral angle-based bending
- [[IPC]] handles frictional [[contact]] for codimensional shells

**Relation to cloth:**
- [[cloth simulation]] is thin shell simulation with specific material parameters (low bending stiffness, anisotropic stretch)

## Papers
- [[Progressive Dynamics for Cloth and Shell Animation]] — coarse-to-fine IPC-based shell simulation
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|A Material Point Method for Thin Shells with Frictional Contact]]
- [[20_Research/Papers/Simulation/Efficient Simulation of Inextensible Cloth|Efficient Simulation of Inextensible Cloth]]
