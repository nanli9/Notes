---
paper_id: "10.1145/1239451.1239500"
title: "Efficient Simulation of Inextensible Cloth"
authors: ["Rony Goldenthal", "David Harmon", "Raanan Fattal", "Michel Bercovier", "Eitan Grinspun"]
year: 2007
venue: "ACM Transactions on Graphics (SIGGRAPH 2007)"
domain: "Simulation"
tags: [paper-notes, Simulation, cloth, inextensibility, constraint, FEM]
quality_score: 9.40
related_papers: ["10.1145/566654.566623", "10.1145/3272127.3275005", "10.1145/2816795.2818081"]
created: "2026-03-22"
status: "analyzed"
---

# Efficient Simulation of Inextensible Cloth

**DOI/arXiv:** [10.1145/1239451.1239500](https://doi.org/10.1145/1239451.1239500) · **Tag:** `10.1145/1239451.1239500`

## Plain-English Summary
Real cloth barely stretches — but enforcing exact inextensibility in simulation is expensive because it turns into a constrained optimization problem. This paper reformulates the inextensibility constraint so it can be solved efficiently within a standard implicit time-stepping framework, avoiding the need for extremely stiff springs (which cause numerical issues) or expensive nonlinear solvers.

## Actual Novelty
The key insight is treating edge-length preservation as a **constraint on velocity** rather than position, which linearizes the problem and allows it to be folded into the implicit integration linear solve. This avoids the classic tradeoff between stiff penalty springs (numerically problematic) and Lagrange multiplier methods (expensive). The result is inextensible [[cloth simulation]] at a cost comparable to standard extensible cloth.

## Method
*(Reconstructed from known details of this well-cited paper)*
1. **Velocity-level constraints:** inextensibility is enforced by constraining the rate of change of edge lengths to zero, rather than constraining lengths directly
2. **Linearization:** the velocity-level formulation is linear in the unknowns, enabling integration into the standard implicit Euler linear system
3. **Fast projection:** a post-step projection corrects any residual stretch, keeping cloth within tolerance of the inextensible manifold
4. Bending is handled separately via standard [[thin shell]] bending models

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Compared against stiff-spring and strain-limiting approaches |
| Metrics | Visual quality, stretch percentage, convergence, timing |
| Runtime | Comparable to standard extensible cloth simulation |
| Failure cases | Not detailed in abstract (abstract unavailable) |

**Note:** Full paper details reconstructed from known properties of this influential work. Abstract was not available in the search results.

## Likely Weaknesses
- **Approximate inextensibility:** the fast projection step means cloth is not exactly inextensible — there's a user-controlled tolerance
- **Edge-based only:** constrains edge lengths, not area — a mesh could theoretically deform in area-changing ways while preserving edges (though unlikely in practice)
- **Interaction with contact:** the velocity-level constraint formulation may interact poorly with [[contact]] constraint methods that also operate at the velocity level
- **2007-era scale:** benchmarks are on meshes that are small by modern standards

## Verdict
A highly influential SIGGRAPH paper that elegantly solved the cloth inextensibility problem. The velocity-level constraint idea is clean and practical — it directly addressed a real pain point in production [[cloth simulation]] where stiff springs caused timestep restrictions. This work influenced many subsequent cloth solvers. While the specific technique has been superseded by more general constraint formulations (e.g., [[IPC]], projective dynamics), the core insight remains relevant.

## Related Papers
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid (2015)]] — solver acceleration for cloth linear systems
- [[20_Research/Papers/Simulation/I-cloth|I-cloth (2018)]] — GPU cloth with incremental collision handling
- [[20_Research/Papers/Simulation/Robust Treatment of Collisions Contact and Friction for Cloth Animation|Robust Collisions (2002)]] — foundational contact handling for cloth
