---
paper_id: "10.1145/2816795.2818081"
title: "Smoothed Aggregation Multigrid for Cloth Simulation"
authors: ["Rasmus Tamstorf", "Toby Jones", "Stephen F. McCormick"]
year: 2015
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, cloth, multigrid, linear-solvers, PCG, contact]
quality_score: 11.65
related_papers: ["10.1145/3528223.3530085", "10.1145/3658214", "10.1145/566654.566623"]
created: "2026-03-22"
status: "analyzed"
---

# Smoothed Aggregation Multigrid for Cloth Simulation

**DOI/arXiv:** [10.1145/2816795.2818081](https://doi.org/10.1145/2816795.2818081) · **Tag:** `10.1145/2816795.2818081`

## Plain-English Summary
Applies algebraic multigrid (specifically smoothed aggregation) as a preconditioner for [[cloth simulation]] solves, replacing geometric multigrid which struggles with unstructured meshes, varying materials, and collision constraints. Introduces prefiltered [[PCG]] for efficiently handling [[contact]] constraints with strong preconditioners.

## Actual Novelty
- First application of smoothed aggregation (SA) multigrid to [[cloth simulation]] — algebraic rather than geometric, so it works on arbitrary tessellations without a mesh hierarchy
- Prefiltered preconditioned conjugate gradient: essential when preconditioners are strong, as standard constrained CG degrades with powerful preconditioners. The prefiltering step projects out constrained DOFs before preconditioning
- Demonstrated on production-scale dressed characters (371k vertices)

## Method
- Smoothed aggregation builds a hierarchy of coarse levels algebraically from the stiffness matrix — no geometric coarsening needed
- Aggregation groups nearby DOFs; prolongation operators are "smoothed" to improve convergence
- Contact constraints handled via prefiltered PCG: constraints are applied to the residual before the preconditioner, preventing the preconditioner from fighting against constraints
- Compatible with implicit [[time integration]] for [[cloth simulation]]

## Evidence Quality

| Aspect | Assessment |
|--------|------------|
| Baselines | Geometric multigrid, standard PCG |
| Metrics | Iteration counts, wall-clock speedups (6–8x on 371k verts) |
| Runtime | 6–8x speedup on production character; larger on synthetic tests |
| Failure cases | Not discussed; AMG setup cost could matter for rapidly changing topology |

## Likely Weaknesses
- Algebraic multigrid setup (computing aggregation + prolongation operators) has non-trivial cost — if mesh connectivity or material properties change every frame, the setup may need recomputation
- SA multigrid can struggle with highly anisotropic problems unless the aggregation strategy accounts for anisotropy (partially addressed but not fully explored)
- The 2015 work predates GPU-native approaches like [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU cloth wrinkles]] and the MAS preconditioner from Wu et al. 2022

## Verdict
Important contribution bringing modern algebraic multigrid to [[cloth simulation]]. The prefiltered PCG idea is the real gem — it's broadly applicable to any constrained solve with strong preconditioners. The 6–8x speedups on production meshes are significant. The algebraic nature makes it robust to mesh quality and anisotropy, which geometric multigrid handles poorly.

## Related Papers
- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics for Cloth and Shell Animation]] — multi-resolution cloth with IPC
- [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU Cloth Wrinkles]] — GPU cloth solver at fine resolution
- [[deformable body]] · [[cloth simulation]] · [[PCG]] · [[linear solvers]] · [[contact]]
