---
paper_id: "10.1145/3306346.3322949"
title: "CD-MPM: Continuum Damage Material Point Method"
authors: ["Joshuah Wolper", "Yu Fang", "Minchen Li", "Jiecong Lu", "Ming Gao", "Chenfanfu Jiang"]
year: 2019
venue: "ACM Transactions on Graphics (SIGGRAPH 2019)"
domain: "Simulation"
tags: [paper-notes, Simulation, fracture, MPM, phase-field, damage-mechanics, plasticity]
quality_score: 16.4
related_papers: ["10.1145/311535.311550", "10.1145/566654.566579", "10.1145/2766896"]
created: "2026-03-19"
status: "analyzed"
---

# CD-MPM: Continuum Damage Material Point Method

**DOI:** [10.1145/3306346.3322949](https://doi.org/10.1145/3306346.3322949) · **Tag:** `10.1145/3306346.3322949`

## Plain-English Summary

Presents two MPM-based approaches for animating dynamic fracture with large elastoplastic deformation, avoiding the mesh-splitting headaches of traditional [[FEM]] fracture. The first approach (**PFF-MPM**) couples a phase-field fracture model with MPM — a damage field evolves via a Ginzburg-Landau equation, softening material where cracks form. The second approach uses a **Non-Associated Cam-Clay (NACC)** plasticity model with a fracture-friendly hardening scheme that naturally produces brittle fracture effects through plasticity alone. Both integrate easily into existing MPM solvers.

## Actual Novelty

1. **Phase-field fracture + MPM:** first coupling of variational phase-field fracture (from computational mechanics) with the Material Point Method for graphics. The phase field $d \in [0,1]$ evolves continuously — no explicit crack tracking, no remeshing.
2. **NACC plasticity model:** novel hardening scheme that makes Cam-Clay produce organic brittle fracture patterns without any explicit fracture criterion. Material simply "falls apart" through plasticity.
3. **Analytical return mapping:** closed-form plasticity return for a wide class of non-associated models — 2× faster than iterative approaches.

## Method

### PFF-MPM (Phase-Field Fracture)
- Damage field $d(x, t) \in [0, 1]$: $d = 0$ intact, $d = 1$ fully broken
- Evolution: Ginzburg-Landau equation — $d$ evolves to minimize total energy (elastic + fracture surface energy)
- Stiffness degradation: $(1 - d)^2 \cdot \psi(\varepsilon)$ — material softens as damage increases
- Length scale parameter $l$ controls crack width (regularization)
- Discretized on MPM grid — coupled momentum + damage solve

### NACC (Non-Associated Cam-Clay)
- Yield surface: elliptical in stress space (pressure vs. deviatoric)
- Novel hardening: softens material post-yield → produces fracture-like separation
- No explicit fracture criterion needed — material behavior alone creates cracks
- Particularly good for: granular materials, ceramics, chalk, cookies

### Both methods:
- No mesh splitting, no remeshing — particles just separate
- Large deformation handled naturally by MPM
- Topology changes are free (particles carry full state)

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Visual comparison to O'Brien 1999, real-world fracture footage |
| Metrics | Visual fidelity, convergence under refinement |
| Runtime | Modest overhead over base MPM (~15-25% for PFF, near-zero for NACC) |
| Failure cases | Phase-field length scale must be tuned; too small = expensive, too large = diffuse cracks |

## Likely Weaknesses

- Phase-field cracks are **diffuse** (smeared over several grid cells) — sharp crack surfaces require post-processing or very fine resolution
- NACC produces organic-looking fracture but with less directional control than stress-tensor methods
- MPM grid resolution limits minimum crack feature size
- No explicit crack surface geometry for rendering — need isosurface extraction from damage field
- Phase-field length scale $l$ is an extra parameter that affects both physics and visual quality

## Verdict

The modern standard for fracture in particle-based simulation. Eliminates the remeshing nightmare of FEM fracture entirely. The PFF approach is more physically principled (variational energy minimization for crack evolution), while NACC is more pragmatic (plasticity-driven, no extra equations). Both produce stunning results. This paper and its follow-ups (anisotropic fracture, Wolper et al. 2020) have made MPM the go-to framework for destruction effects in research. Highly recommended for anyone building fracture systems — much simpler to implement than FEM mesh splitting.

## Related Papers

- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|O'Brien & Hodgins 1999]] — foundational FEM fracture this supersedes
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Ductile Fracture|O'Brien, Bargteil & Hodgins 2002]] — ductile FEM fracture
- [[20_Research/Papers/Simulation/High-Resolution Brittle Fracture Simulation with Boundary Elements|Hahn & Wojtan 2015]] — BEM alternative for high-resolution cracks
