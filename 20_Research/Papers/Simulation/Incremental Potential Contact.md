---
paper_id: "10.1145/3386569.3392425"
title: "Incremental Potential Contact"
authors: ["Minchen Li", "Zachary Ferguson", "Teseo Schneider", "Timothy R. Langlois", "Denis Zorin", "Daniele Panozzo", "Chenfanfu Jiang", "Danny M. Kaufman"]
year: 2020
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, IPC, contact, friction, FEM, implicit-integration, foundational]
quality_score: 21.40
related_papers: ["10.1145/3450626.3459767", "10.1145/3658214", "10.1145/3734518"]
created: "2026-03-21"
status: "analyzed"
---

# Incremental Potential Contact

**DOI/arXiv:** [10.1145/3386569.3392425](https://doi.org/10.1145/3386569.3392425) · **Tag:** `10.1145/3386569.3392425`

## Plain-English Summary

[[IPC]] solves the longstanding problem of robust frictional contact in implicit elastodynamics. It formulates contact as a smooth barrier energy added to the incremental potential (the variational form of implicit time stepping), ensuring that the optimization never allows intersections or inversions. A custom nonlinear solver with continuous collision detection (CCD) line search guarantees feasibility at every iterate. The method provides separate user-controlled tolerances for dynamics accuracy and geometric contact accuracy.

## Actual Novelty

Three key contributions: (1) a smooth barrier potential for [[contact]] that goes to infinity as distance approaches zero, integrated into the variational time-stepping objective; (2) a CCD-based line search that guarantees intersection-free trajectories at every solver iteration, not just at converged solutions; (3) decoupled accuracy controls for physical dynamics vs. geometric surface conformity. The barrier formulation is the critical innovation — unlike penalty methods, it provides a strict mathematical guarantee of non-intersection, and unlike constraint methods, it fits naturally into optimization-based [[time integration]].

## Method

1. Standard implicit Euler formulated as incremental potential minimization: kinetic + elastic + external energies
2. Barrier energy $b(d)$ added for every close contact pair, where $b \to \infty$ as distance $d \to 0$
3. Friction modeled via a smoothed semi-implicit dissipation potential (lagged normal force)
4. Custom Newton-type solver with:
   - CCD-filtered line search (ensures no intersection at any iterate)
   - Adaptive barrier stiffness scaling
   - Convergence criteria for both dynamics ($\epsilon_d$) and contact gap ($\hat{d}$)
5. Works with [[FEM]] tet meshes; uses vertex-face and edge-edge proximity pairs

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Extensive comparison against commercial codes (Abaqus), research libraries (ARCSim, SOFA), and engineering methods |
| Metrics | Intersection counts, energy convergence, constraint violation, timing |
| Runtime | Up to 2.3M tet meshes, 498K contacts/step; practical but not real-time |
| Failure cases | Acknowledged: computational cost scales with contact density; friction is lagged (semi-implicit) |

## Likely Weaknesses

- **Performance**: the barrier formulation and CCD line search impose significant computational overhead — not suitable for real-time applications
- **Friction model is lagged**: normal forces from previous iteration used for friction, so friction is not fully implicit (addressed partially in later work)
- **Mesh dependency**: requires surface triangulations; does not natively handle codimensional structures (addressed by [[20_Research/Papers/Simulation/Codimensional Incremental Potential Contact|C-IPC]])
- **Barrier stiffness tuning**: while adaptive, the barrier parameter $\hat{d}$ still requires user specification and affects both accuracy and performance
- **CCD cost**: continuous collision detection is expensive and can dominate runtime in dense-contact scenarios

## Verdict

A landmark paper that fundamentally changed how the graphics community approaches contact simulation. The variational barrier formulation is mathematically elegant and provides the strongest feasibility guarantees of any implicit contact method. It has become the de facto standard for robust contact in offline simulation, spawning an extensive family of follow-up work (C-IPC, differentiable IPC, ACCD). The main limitation — computational cost — has been partially addressed by subsequent GPU implementations and the Reliable Iterative Dynamics solver. Essential reading for anyone working on [[contact]], [[cloth simulation]], or [[deformable body]] simulation.

## Related Papers

- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics]] — uses IPC as its contact model
- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]] — faster solver compatible with IPC
- [[20_Research/Papers/Simulation/Codimensional Incremental Potential Contact|C-IPC]] — extension to codimensional structures
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|MPM Thin Shells]] — alternative continuum contact approach
