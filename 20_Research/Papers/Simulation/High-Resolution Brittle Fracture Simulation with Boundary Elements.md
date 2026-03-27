---
paper_id: 10.1145/2766896
title: High-Resolution Brittle Fracture Simulation with Boundary Elements
authors:
  - David Hahn
  - Chris Wojtan
year: 2015
venue: ACM Transactions on Graphics
domain: Simulation
tags:
  - paper-notes
  - Simulation
  - fracture
  - BEM
  - brittle
  - LEFM
  - crack-front
quality_score: 13.65
related_papers:
  - 10.1145/311535.311550
  - 10.1145/2897824.2925902
created: 2026-03-19
status: analyzed
---

# High-Resolution Brittle Fracture Simulation with Boundary Elements

**DOI:** [10.1145/2766896](https://doi.org/10.1145/2766896) · **Tag:** `10.1145/2766896`

## Plain-English Summary

Uses the **Boundary Element Method (BEM)** instead of FEM to simulate brittle [[fracture simulation|fracture]] under quasi-static Linear Elastic Fracture Mechanics (LEFM). The key advantage: BEM only discretizes the surface (not the volume), enabling extremely detailed fracture surfaces. A low-resolution BEM mesh computes stress intensity factors, which are then interpolated onto a high-resolution Lagrangian crack-front for propagation. This decoupling of computational and visual resolution produces physics-based fracture surfaces with unprecedented detail.

## Actual Novelty

Separating crack-front resolution from computational mesh resolution. Previous FEM fracture methods tied crack detail to simulation resolution — fine cracks required fine volumetric meshes (cubic cost increase). BEM naturally operates on surfaces, and this paper exploits that by running a coarse BEM solve but propagating a fine crack-front via interpolated stress intensity factors. Also enables spatial variation of material toughness/strength for controlled artistic effects.

## Method

1. **BEM formulation:** solve elasticity equations on the surface only — boundary integral equation relates surface tractions to displacements
2. Compute **stress intensity factors** $K_I, K_{II}, K_{III}$ at the crack front from BEM solution
3. **Crack propagation:** advance the high-resolution Lagrangian crack-front based on interpolated SIFs
   - Crack speed and direction determined by $K_I$ (opening mode) and material toughness $K_{IC}$
   - Crack advances where $K_I > K_{IC}$
4. **Multi-resolution:** coarse BEM mesh (~100s of elements) drives fine crack-front (~1000s of points)
5. Matrix re-use: previously computed blocks of BEM system matrix can be recycled as crack grows
6. Crack initiation handled separately from propagation — can use stress criterion or user-specified initiation points

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to FEM fracture (O'Brien 1999 lineage) |
| Metrics | Visual detail of fracture surfaces, convergence, timing |
| Runtime | BEM solve is $O(n^2)$ to $O(n^3)$ — expensive per step but fewer DOFs than volumetric FEM |
| Failure cases | Quasi-static assumption breaks for very dynamic impacts |

## Likely Weaknesses

- **Quasi-static assumption:** no inertial effects — not suitable for high-speed impacts where wave propagation matters
- BEM scales as $O(n^2)$ in memory and $O(n^3)$ in time (dense system matrix) — limits total DOFs
- Linear elasticity only — no plasticity, no large deformation
- Crack-front tracking is geometrically complex (crack branching, merging)
- No coupling to dynamics solver for rigid body interaction (addressed in 2016 follow-up)

## Verdict

Beautiful work that exploits BEM's natural surface-only discretization to produce fracture detail that would be prohibitively expensive with FEM. The multi-resolution trick (coarse solve, fine crack) is elegant. Best suited for offline rendering of detailed fracture surfaces rather than real-time destruction. The 2016 follow-up (fast approximations) addresses the performance issues and adds rigid body coupling.

## Related Papers

- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|O'Brien & Hodgins 1999]] — FEM fracture this builds on
- [[20_Research/Papers/Simulation/Fast Approximations for Boundary Element Based Brittle Fracture Simulation|Fast BEM Fracture (Hahn & Wojtan 2016)]] — faster follow-up with rigid body coupling
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]] — MPM alternative (meshfree, handles large deformation)
