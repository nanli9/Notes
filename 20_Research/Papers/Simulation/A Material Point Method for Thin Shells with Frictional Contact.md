---
paper_id: "10.1145/3197517.3201346"
title: "A Material Point Method for Thin Shells with Frictional Contact"
authors: ["Qi Guo", "Xuchen Han", "Chuyuan Fu", "Theodore Gast", "Rasmus Tamstorf", "Joseph Teran"]
year: 2018
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, MPM, thin-shell, contact, friction, cloth]
quality_score: 21.90
related_papers: ["10.1145/2461912.2461948", "10.1145/3450626.3459767", "10.1145/3386569.3392425"]
created: "2026-03-21"
status: "analyzed"
---

# A Material Point Method for Thin Shells with Frictional Contact

**DOI/arXiv:** [10.1145/3197517.3201346](https://doi.org/10.1145/3197517.3201346) · **Tag:** `10.1145/3197517.3201346`

## Plain-English Summary

Instead of using traditional collision detection and response for shells and cloth, this paper embeds thin shells into a continuum [[MPM]] framework. The shell's through-thickness behavior is modeled with a continuum constitutive model that separates bending (Kirchhoff-Love) from contact/shearing. Frictional contact is resolved automatically via an elastoplastic model on the shear/compression components — no collision detection or response step is needed. This lets the method handle hundreds of thousands of DOFs with complex contacts in minutes per frame.

## Actual Novelty

The key insight is treating [[contact]] and friction as a continuum elastoplastic phenomenon rather than a discrete constraint. By decomposing shell kinematics into a Kirchhoff-Love rotation plus shearing/compression along the normal, the method decouples bending from contact resistance. The elastoplastic constitutive model on the normal-direction components acts as an implicit contact/friction solver. This is genuinely novel — prior [[MPM]] work did not handle [[thin shell]] structures, and prior [[cloth simulation]] methods required explicit collision detection.

## Method

1. Shell mid-surface discretized with subdivision elements for bending (Kirchhoff-Love)
2. Through-thickness behavior captured by [[MPM]] particles carrying a continuum deformation state
3. Deformation decomposed: mid-surface normal rotation (bending) + shear/compression (contact)
4. Elastoplastic constitutive model on shear/compression: elastic resistance prevents interpenetration, plasticity models friction (Coulomb-like yield surface)
5. Standard [[MPM]] grid transfers handle the Eulerian contact resolution
6. Naturally couples with granular/fluid MPM materials

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Compared qualitatively to standard cloth sims; no direct quantitative baseline against IPC or penalty methods |
| Metrics | Visual plausibility, DOF counts, timing |
| Runtime | "Few minutes per frame" for 100K–1M DOF examples |
| Failure cases | Not explicitly discussed; implicit assumption that MPM grid resolution must be sufficient relative to shell thickness |

## Likely Weaknesses

- No explicit convergence study or quantitative comparison against established [[contact]] methods
- Accuracy depends on MPM grid resolution relative to shell thickness — very thin shells may need excessive grid refinement
- The continuum contact model is approximate: no formal guarantee of intersection-free trajectories (unlike [[IPC]])
- Runtime ("few minutes per frame") is not competitive with modern GPU cloth solvers for equivalent quality
- Bending model limited to Kirchhoff-Love; no Reissner-Mindlin comparison for thick shells

## Verdict

An elegant reformulation that eliminates collision detection entirely for thin shells by absorbing contact into the MPM continuum framework. The idea of contact-as-plasticity is genuinely creative and influential — it directly motivated follow-up work on codimensional [[MPM]] and hybrid FEM-MPM coupling. However, the lack of intersection-free guarantees and quantitative benchmarks against dedicated contact methods (like [[IPC]]) limits its applicability for production scenarios requiring strict physical accuracy. Best suited for scenarios with massive contact (granular-shell coupling, crumpling) where traditional CD is prohibitively expensive.

## Related Papers

- [[20_Research/Papers/Simulation/A Material Point Method for Snow Simulation|A Material Point Method for Snow Simulation]] — foundational MPM paper
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]] — intersection-free alternative approach to contact
- [[20_Research/Papers/Simulation/Codimensional Incremental Potential Contact|C-IPC]] — extends IPC to codimensional structures
