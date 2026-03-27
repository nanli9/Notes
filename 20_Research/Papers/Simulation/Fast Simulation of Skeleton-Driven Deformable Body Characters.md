---
paper_id: "10.1145/2019627.2019640"
title: "Fast Simulation of Skeleton-Driven Deformable Body Characters"
authors: ["Junggon Kim", "Nancy S. Pollard"]
year: 2011
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, Animation, deformable, skinning, skeleton, FEM, reduced-order, real-time]
quality_score: 10.9
related_papers: ["10.1145/3355089.3356486", "10.1145/1531326.1531359"]
created: "2026-03-19"
status: "analyzed"
---

# Fast Simulation of Skeleton-Driven Deformable Body Characters

**DOI:** [10.1145/2019627.2019640](https://doi.org/10.1145/2019627.2019640) · **Tag:** `10.1145/2019627.2019640`

## Plain-English Summary

A fast physics-based simulation system for characters with both a skeleton and a [[deformable body]]. Handles two-way interaction: the skeleton drives the deformable body, the deformable body's dynamics feed back into the skeleton, and both interact with the environment. Can also compute passive jiggling (secondary motion) from kinematic skeletal animation. Achieves real-time or near-real-time by combining: reduced [[FEM]] with nonlinear elements, linear-time skeleton dynamics, explicit integration, and GPU parallelism.

## Actual Novelty

The coordinated combination of three acceleration strategies that individually are known but together achieve orders-of-magnitude speedup over existing methods while preserving accuracy: (1) reduced deformable model with nonlinear FEM (not just linear), (2) O(n) articulated body algorithm for skeleton, (3) explicit time integration (viable because the reduced model is well-conditioned). The two-way coupling between skeleton and deformable body through dynamics is the main contribution — prior work either did kinematic driving or one-way coupling.

## Method

- **Reduced deformable body:** project FEM onto low-dimensional subspace, but retain nonlinear strain for large deformation accuracy
- **Skeleton dynamics:** linear-time recursive Newton-Euler algorithm for articulated rigid bodies
- **Coupling:** skeleton forces → deformable body surface, deformable body reaction forces → skeleton joints
- **Explicit integration:** stable because reduced model has bounded eigenvalues
- **GPU acceleration:** parallel force computation for complex characters
- Applications: self-propelled deformable characters, passive jiggling on kinematic skeletons

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to full FEM + skeleton simulation |
| Metrics | Timing, visual quality, physical accuracy of deformation |
| Runtime | Real-time to near-real-time for tested characters |
| Failure cases | Extreme deformations may exceed reduced basis expressiveness |

## Likely Weaknesses

- Reduced basis must be precomputed — new characters need offline basis construction
- Explicit integration limits maximum stiffness (can't handle very stiff materials)
- Two-way coupling accuracy depends on quality of the reduced basis
- 2011 GPU implementation — modern GPU architectures could push further

## Verdict

Practical system for physics-based character animation with deformable bodies. The engineering is solid — each component is chosen for its speed/accuracy trade-off in this specific context. Particularly useful for game-like scenarios where characters need physically plausible secondary motion (jiggling, wobbling) without offline simulation. Bridges the gap between pure [[skinning]] (fast but no physics) and full simulation (accurate but slow).

## Related Papers

- [[20_Research/Papers/Simulation/A Scalable Galerkin Multigrid Method for Real-time Simulation of Deformable Objects|Galerkin Multigrid Deformable Sim]] — alternative real-time deformable solver
- [[20_Research/Papers/Simulation/Deformable Object Animation Using Reduced Optimal Control|Reduced Optimal Control]] — related reduced-order deformable approach
