---
title: "Deformable Body Simulation"
type: topic
tags: [topic, Simulation, deformable, FEM, elasticity]
---

# Deformable Body Simulation

Simulation of objects that change shape under forces — includes cloth, soft bodies, elastic solids, and biological tissue.

**Categories:**
- Codimensional (cloth, shells, rods) — [[cloth simulation]], [[thin shell]]
- Volumetric (soft bodies, organs) — [[FEM]]-based, [[MPM]]
- Rigid-deformable coupling

**Common methods:**
- [[FEM]] — accurate continuum mechanics, expensive
- [[MPM]] — handles topology change, fracture, fluid coupling
- [[PBD]] / XPBD — real-time, position-based
- Shape matching / clustered shape matching — real-time, robust
- Reduced-order methods — low-dim subspace for speed

## Papers
- [[Progressive Dynamics for Cloth and Shell Animation]]
- [[Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]] — RT for deforming geometry
- [[Deformable Object Animation Using Reduced Optimal Control]] — reduced-order space-time optimization
- [[20_Research/Papers/Simulation/Fast Simulation of Skeleton-Driven Deformable Body Characters|Fast Simulation of Skeleton-Driven Deformable Body Characters]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains|A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains]]
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
- [[20_Research/Papers/Simulation/SoftMimicGen|SoftMimicGen]]
