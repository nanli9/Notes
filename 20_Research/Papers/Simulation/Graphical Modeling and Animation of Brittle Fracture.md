---
paper_id: "10.1145/311535.311550"
title: "Graphical Modeling and Animation of Brittle Fracture"
authors: ["James F. O'Brien", "Jessica K. Hodgins"]
year: 1999
venue: "SIGGRAPH 1999"
domain: "Simulation"
tags: [paper-notes, Simulation, fracture, FEM, brittle, foundational, stress-tensor]
quality_score: 12.95
related_papers: ["10.1145/566654.566579", "10.1145/2766896"]
created: "2026-03-19"
status: "analyzed"
---

# Graphical Modeling and Animation of Brittle Fracture

**DOI/arXiv:** [10.1145/311535.311550](https://doi.org/10.1145/311535.311550) · **Tag:** `10.1145/311535.311550`
**Also available:** [arxiv:2303.02934](https://arxiv.org/abs/2303.02934)

## Plain-English Summary

The foundational paper for physics-based [[fracture simulation]] in computer graphics. Augments [[FEM]] simulation of flexible objects with models for crack initiation and propagation in 3D volumes. The simulation analyzes stress tensors computed over the finite element mesh, determines where cracks should start (where maximum principal stress exceeds material strength), and in what direction they should propagate (perpendicular to the direction of maximum tension). Demonstrated with breaking bowls, cracking walls, and colliding fracturing objects. Received the **SIGGRAPH 99 Impact Award** and contributed to James O'Brien's **Academy Award (2015)** for technical achievement.

## Actual Novelty

First complete framework for stress-driven fracture in graphics. Previous work either used pre-scored geometry or simple spring-breaking. This paper introduces the full pipeline: [[FEM]] stress computation → eigendecomposition of Cauchy stress tensor → principal-stress-based crack initiation → crack plane insertion → dynamic remeshing. The key insight is using the principal stress directions to determine physically correct crack orientation rather than following mesh edges.

## Method

1. Simulate deformable body using tetrahedral [[FEM]] with linear elasticity
2. At each time step, compute **Cauchy stress tensor** $\sigma$ per element
3. **Eigendecompose** $\sigma$ → principal stresses $\sigma_1 \geq \sigma_2 \geq \sigma_3$ and principal directions $\hat{n}_1, \hat{n}_2, \hat{n}_3$
4. **Crack initiation:** if $\sigma_1 > \sigma_{\text{crit}}$ (material tensile strength) in any element:
   - Crack plane passes through element centroid
   - Crack normal = $\hat{n}_1$ (perpendicular to maximum tension)
5. **Mesh splitting:** duplicate nodes along crack surface, re-tetrahedralize locally
6. Continue simulation with updated topology
7. Material properties (Young's modulus, Poisson's ratio, tensile strength) control behavior — varying them produces effects from shattering glass to clean concrete breaks

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | No prior comparable method existed — this is the baseline |
| Metrics | Visual plausibility, physical reasoning; no quantitative validation against real fracture |
| Runtime | Not real-time; offline simulation (1999 hardware) |
| Failure cases | Remeshing can produce degenerate elements; crack path is mesh-resolution-dependent |

## Likely Weaknesses

- Remeshing is the Achilles' heel: splitting tetrahedra and maintaining mesh quality is fragile and expensive
- Linear elasticity assumption — real materials exhibit nonlinear stress-strain before fracture
- Crack paths are somewhat resolution-dependent (finer mesh → more detailed cracks, but also more expensive)
- No plastic deformation before fracture (purely brittle) — addressed in the 2002 ductile follow-up
- No adaptive refinement near crack tips where stress gradients are steepest

## Verdict

**The paper** for fracture in graphics. Everything that followed — ductile fracture, BEM fracture, MPM fracture, Voronoi pre-fracture — builds on or responds to this work. The stress-tensor-driven crack initiation framework is still the conceptual foundation 25+ years later. Must-read for anyone working on destruction effects. The method was commercialized as Digital Molecular Matter (Pixelux), used in 200+ films.

## Related Papers

- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Ductile Fracture|Graphical Modeling and Animation of Ductile Fracture]] — direct sequel by O'Brien, Bargteil & Hodgins (2002)
- [[20_Research/Papers/Simulation/High-Resolution Brittle Fracture Simulation with Boundary Elements|High-Resolution Brittle Fracture]] — Hahn & Wojtan (2015), BEM alternative
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]] — Wolper et al. (2019), MPM-based fracture
