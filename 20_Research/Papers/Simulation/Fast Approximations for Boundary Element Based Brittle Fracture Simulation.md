---
paper_id: "10.1145/2897824.2925902"
title: "Fast Approximations for Boundary Element Based Brittle Fracture Simulation"
authors: ["David Hahn", "Chris Wojtan"]
year: 2016
venue: "ACM Transactions on Graphics (SIGGRAPH 2016)"
domain: "Simulation"
tags: [paper-notes, Simulation, fracture, BEM, brittle, rigid-body-coupling]
quality_score: 13.65
related_papers: ["10.1145/2766896", "10.1145/311535.311550"]
created: "2026-03-19"
status: "analyzed"
---

# Fast Approximations for Boundary Element Based Brittle Fracture Simulation

**DOI:** [10.1145/2897824.2925902](https://doi.org/10.1145/2897824.2925902) · **Tag:** `10.1145/2897824.2925902`

## Plain-English Summary

Follow-up to Hahn & Wojtan 2015 that makes BEM-based [[fracture simulation]] significantly faster. Introduces simplifying assumptions for stress intensity and opening displacement estimation so that each time step scales **linearly** with crack-front length (vs. cubic for full BEM). Also integrates fracture with a standard **rigid-body solver** — objects crack on impact and the resulting fragments become rigid bodies. A hybrid approach uses expensive full BEM while DOF count is low, then switches to the fast approximation as crack geometry becomes complex.

## Actual Novelty

1. **Linear-time crack propagation:** by approximating stress intensities locally rather than solving the full BEM system, each step is $O(n)$ in crack-front length
2. **BEM→fast transition:** hybrid that gracefully degrades from accurate to approximate as complexity grows (user-defined threshold)
3. **Rigid-body coupling:** Neumann boundary value problem separates translational, rotational, and deformational force components with Tikhonov regularization — first integration of BEM fracture with standard physics engines

## Method

1. Start with full BEM solve for initial crack propagation (accurate, expensive)
2. When DOF count exceeds threshold → switch to fast approximation:
   - Local stress intensity estimation from nearby crack geometry
   - Opening displacements approximated from local stress state
   - Per-step cost: $O(n)$ in crack-front length
3. **Rigid-body coupling:**
   - Collision forces decomposed into: translation + rotation + deformation
   - Deformation component drives fracture via BEM/fast solver
   - Fragments handed off to rigid body solver after separation
   - Tikhonov regularization stabilizes the force decomposition

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Full BEM (2015 paper), FEM fracture |
| Metrics | Visual quality comparison, timing, fracture pattern similarity |
| Runtime | Orders of magnitude faster than full BEM; handles complex scenes |
| Failure cases | Approximation degrades for very curved or branching cracks |

## Likely Weaknesses

- Fast approximation sacrifices accuracy — crack paths may differ from full BEM solution
- Still quasi-static LEFM — no dynamic stress waves
- Rigid-body handoff is one-way (fragment can't re-fracture easily in the rigid body phase)
- Tikhonov regularization parameter needs tuning for stability vs. accuracy

## Verdict

Practical and production-friendly version of BEM fracture. The linear-time approximation makes it viable for complex scenes, and rigid-body coupling makes it useful in actual animation pipelines. Together with the 2015 paper, this pair represents the strongest BEM-based fracture framework in graphics. If you want detailed crack surfaces with reasonable performance and don't need large plastic deformation, this is the method.

## Related Papers

- [[20_Research/Papers/Simulation/High-Resolution Brittle Fracture Simulation with Boundary Elements|High-Resolution BEM Fracture (2015)]] — predecessor with full accuracy
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|O'Brien & Hodgins 1999]] — FEM fracture baseline
