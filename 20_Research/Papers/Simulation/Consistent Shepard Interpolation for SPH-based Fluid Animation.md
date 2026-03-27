---
paper_id: "10.1145/3355089.3356503"
title: "Consistent Shepard Interpolation for SPH-based Fluid Animation"
authors: ["Stefan Reinhardt", "Tim Krake", "Bernhard Eberhardt", "Daniel Weiskopf"]
year: 2019
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, fluid, SPH, kernel-correction, density-estimation]
quality_score: 13.15
related_papers: ["10.1145/1857907.1857912", "10.1145/1073368.1073400"]
created: "2026-03-19"
status: "analyzed"
---

# Consistent Shepard Interpolation for SPH-based Fluid Animation

**DOI/arXiv:** [10.1145/3355089.3356503](https://doi.org/10.1145/3355089.3356503) · **Tag:** `10.1145/3355089.3356503`

## Plain-English Summary

[[SPH]] fluid simulations suffer from density estimation errors near free surfaces and at irregular particle distributions. The standard Shepard kernel correction normalizes the smoothing kernel but uses uncorrected densities to compute the correction factor — a circular inconsistency. This paper fixes this by formulating a consistent Shepard correction where corrected densities are used throughout, turning it into an implicit problem solved efficiently via the power method. The result: orders-of-magnitude smoother density fields.

## Actual Novelty

Identifies and fixes a fundamental inconsistency in the standard Shepard correction for SPH. The insight is simple but the impact is significant: using uncorrected densities to compute the correction factor propagates the very errors you're trying to fix. The implicit formulation + power method solver is clean and practical.

## Method

- Standard Shepard correction: normalize kernel by `∑ W / ρ` — but `ρ` itself was computed with uncorrected kernel
- Consistent formulation: use corrected `ρ` in the normalization denominator → implicit equation
- Solved via the power method (iterative, converges in 2–4 iterations typically)
- Applied to both WCSPH (weakly compressible) and DFSPH (divergence-free SPH)
- Extended to rigid-fluid coupling density estimation

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Standard SPH, standard Shepard correction, DFSPH |
| Metrics | Density field noise (orders of magnitude reduction shown), visual quality |
| Runtime | Power method adds ~2–4 iterations per step; overhead described as modest |
| Failure cases | Not discussed in detail |

## Likely Weaknesses

- The power method iterations add per-step cost — quantitative overhead benchmarks would strengthen the claim
- Demonstrated primarily on density smoothness; less clear how much downstream dynamics (pressure forces, surface tracking) actually improve
- No comparison with other kernel correction methods (e.g., MLS-based corrections)
- Limited to SPH variants; not applicable to grid-based or hybrid methods

## Verdict

Elegant fix for a well-known SPH artifact. The inconsistency in standard Shepard correction is a real issue that practitioners often paper over with damping or smaller time steps. The power method solution is easy to implement and the density improvement is dramatic. Good reference for anyone implementing [[SPH]] fluid solvers, though the practical impact on final animation quality (vs. just density plots) could be better demonstrated.

## Related Papers

- [[20_Research/Papers/Simulation/A PML-based Nonreflective Boundary for Free Surface Fluid Animation|PML Boundary for Fluid]] — another fluid accuracy technique (boundary conditions)
