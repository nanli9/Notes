---
title: "Contact and Collision Handling"
type: topic
tags: [topic, Simulation, contact, collision, friction]
---

# Contact and Collision Handling

Detecting and resolving interpenetrations between simulated objects. One of the hardest problems in physics-based animation.

**Detection:**
- Continuous collision detection (CCD) — detects first time of impact along a trajectory
- Spatial hashing, [[BVH]] hierarchies — broad phase
- Triangle–triangle, vertex–face, edge–edge — narrow phase

**Resolution methods:**
- Penalty forces — simple, prone to tunneling
- Impulse-based (LCP) — standard in rigid body; hard for deformables
- Position correction ([[PBD]]) — fast, approximate
- [[IPC]] (barrier energy) — provably intersection-free, expensive

**Friction:**
- Coulomb model — static / kinetic threshold
- Anisotropic friction — direction-dependent (important for cloth on skin)
- Measurement-based models — derived from real fabric experiments

## Course Notes

### Penalty Method
- $F = k \cdot d \cdot N$ (spring-like repulsion proportional to penetration depth)
- **Limitations:** tunneling (objects pass through at high velocity), sticking (residual force when nearly touching)
- Simple to implement but not robust for thin objects like cloth

### Incremental Potential Contact ([[IPC]])
- Barrier energy approach: energy $E$ increases steeply as distance $d \to 0$
- Energy-distance curve: smooth, monotonically increasing as $\varepsilon \to 0$
- Guarantees intersection-free by construction (objects never reach $d = 0$)

## Papers
- [[Robust Treatment of Collisions, Contact and Friction for Cloth Animation]] — Bridson et al., foundational cloth contact
- [[Progressive Dynamics for Cloth and Shell Animation]] — IPC-based multi-level contact
- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]]
- [[20_Research/Papers/Simulation/Simplicits|Simplicits]]
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|A Material Point Method for Thin Shells with Frictional Contact]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains|A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains]]
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
- [[20_Research/Papers/Simulation/Efficient Simulation of Inextensible Cloth|Efficient Simulation of Inextensible Cloth]]
- [[20_Research/Papers/Simulation/I-cloth|I-cloth]]
- [[20_Research/Papers/Simulation/Characterizing Long-Range Dependencies in Knee Joint Contact Mechanics|Characterizing Long-Range Dependencies in Knee Joint Contact Mechanics]]
- [[20_Research/Papers/Simulation/Geometrically Plausible Object Pose Refinement using Differentiable Simulation|Geometrically Plausible Object Pose Refinement using Differentiable Simulation]]
- [[20_Research/Papers/Animation/Unleashing Guidance Without Classifiers for Human-Object Interaction Animation|Unleashing Guidance Without Classifiers for Human-Object Interaction Animation]]
