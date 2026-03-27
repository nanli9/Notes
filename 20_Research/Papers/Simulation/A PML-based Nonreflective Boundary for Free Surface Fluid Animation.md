---
paper_id: "10.1145/1857907.1857912"
title: "A PML-based Nonreflective Boundary for Free Surface Fluid Animation"
authors: ["Andreas Söderström", "Matts Karlsson", "Ken Museth"]
year: 2010
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, fluid, PML, boundary-conditions, free-surface]
quality_score: 14.65
related_papers: ["10.1145/1141911.1141961", "10.1145/3355089.3356503"]
created: "2026-03-19"
status: "analyzed"
---

# A PML-based Nonreflective Boundary for Free Surface Fluid Animation

**DOI/arXiv:** [10.1145/1857907.1857912](https://doi.org/10.1145/1857907.1857912) · **Tag:** `10.1145/1857907.1857912`

## Plain-English Summary

When simulating water (e.g. ocean waves around a ship), waves hitting the edge of the simulation domain bounce back and create unrealistic interference patterns. This paper adapts the Perfectly Matched Layer (PML) technique — well-known in computational electromagnetics — to the incompressible Euler/Navier-Stokes equations with free surfaces. The PML boundary absorbs outgoing waves, preventing reflections. The solver extends the stable-fluids approach for fast, stable integration of the PML equations.

## Actual Novelty

First application of PML absorbing boundaries to free-surface incompressible fluid animation in graphics. The formulation handles both velocity and pressure fields at the boundary, derived specifically for the Navier-Stokes equations rather than just the wave equation. The key contribution is showing PML can be made compatible with the projection-based (stable fluids) solver framework commonly used in graphics.

## Method

- Derives PML damping terms for the incompressible Euler and Navier-Stokes equations
- Splits velocity and pressure fields into physical and auxiliary components at the boundary layer
- Solves the augmented system using a modified stable-fluids pipeline (advection → external forces → PML damping → pressure projection)
- PML layer width and damping profile are user-controllable parameters

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared against: open boundary, clamped boundary, sponge layer |
| Metrics | Wave amplitude reflection ratio, visual comparison |
| Runtime | Not explicitly benchmarked vs. base solver; described as "fast and stable" |
| Failure cases | Deep-water waves with very long wavelengths may require wider PML layers |

## Likely Weaknesses

- Runtime overhead of PML not quantified — auxiliary field doubles unknowns at the boundary
- Parameter tuning (layer width, damping profile) requires experimentation per scene
- Only demonstrated on uniform grid; unclear how this adapts to adaptive or particle-based methods like [[SPH]]
- 2010 paper — modern GPU solvers may need a different formulation

## Verdict

Clean adaptation of a proven absorbing-boundary technique to graphics fluid sim. Solves a real pain point (boundary reflections in open-water scenes). The PML formulation is well-grounded in physics, unlike ad-hoc damping zones. Useful reference for anyone implementing open-domain [[fluid simulation]] on uniform grids.

## Related Papers

- [[20_Research/Papers/Simulation/Consistent Shepard Interpolation for SPH-based Fluid Animation|Consistent Shepard Interpolation]] — another fluid sim accuracy improvement
- [[20_Research/Papers/Simulation/Fluid Animation with Dynamic Meshes|Fluid Animation with Dynamic Meshes]] — alternative fluid framework (tet meshes)
