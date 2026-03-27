---
title: "IPC — Incremental Potential Contact"
type: topic
tags: [topic, Simulation, contact, deformable]
---

# IPC — Incremental Potential Contact

A contact and friction simulation framework that guarantees intersection-free trajectories and convergent optimization. Introduced by Li et al. (SIGGRAPH 2020). Uses a barrier energy formulation that prevents interpenetration by construction — the barrier activates as objects approach, keeping them apart without a hard constraint solve.

**Key properties:**
- Provably intersection-free at every step
- Handles complex frictional contact (sliding, sticking, rolling)
- Works for cloth, shells, volumetric deformables, and rigid bodies
- Computationally expensive (implicit, Newton-based) — not real-time

**Relation to other methods:**
- Supersedes penalty-based and impulse-based contact for high-fidelity offline sim
- [[cloth simulation]] pipelines built on IPC (e.g. [[Progressive Dynamics for Cloth and Shell Animation]])
- Lighter alternatives: [[PBD]], [[contact|projective dynamics]]

## Papers
- [[Progressive Dynamics for Cloth and Shell Animation]]
- [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels]]
- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]]
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|A Material Point Method for Thin Shells with Frictional Contact]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/Efficient Simulation of Inextensible Cloth|Efficient Simulation of Inextensible Cloth]]
- [[20_Research/Papers/Simulation/Geometrically Plausible Object Pose Refinement using Differentiable Simulation|Geometrically Plausible Object Pose Refinement using Differentiable Simulation]]
