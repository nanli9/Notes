---
paper_id: "10.1145/2461912.2461948"
title: "A Material Point Method for Snow Simulation"
authors: ["Alexey Stomakhin", "Craig Schroeder", "Lawrence Chai", "Joseph Teran", "Andrew Selle"]
year: 2013
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, MPM, snow, elasto-plastic, constitutive-model, foundational]
quality_score: 24.4
related_papers: ["10.1145/3306346.3322949", "10.1145/2897824.2925877", "10.1145/3731155"]
created: "2026-03-20"
status: "analyzed"
---

# A Material Point Method for Snow Simulation

**DOI:** [10.1145/2461912.2461948](https://doi.org/10.1145/2461912.2461948) · **Tag:** `10.1145/2461912.2461948`

## Plain-English Summary

The foundational paper that brought [[MPM]] (Material Point Method) to mainstream computer graphics, developed at Disney for *Frozen*. Snow exhibits both solid-like behavior (packing, supporting weight) and fluid-like behavior (flowing, avalanching), making it extremely difficult to simulate with pure solid or fluid methods. MPM's hybrid Eulerian/Lagrangian approach naturally handles this duality: Lagrangian particles carry material state while an Eulerian grid handles collision and integration.

## Actual Novelty

The combination of (1) a user-controllable elasto-plastic constitutive model for snow with (2) MPM's hybrid particle-grid framework for graphics-quality simulation. The constitutive model captures the critical hardening behavior of snow (compressed snow becomes stiffer) and the fracture/flow transition. The Cartesian grid automates self-collision and fracture without explicit detection. Semi-implicit integration has conditioning independent of particle count.

## Method

- **Hybrid Lagrangian/Eulerian:** particles carry mass, velocity, deformation gradient; grid handles forces and integration
- **Elasto-plastic constitutive model:**
  - Elastic: co-rotational with hardening (stiffness increases under compression)
  - Plastic: deformation gradient clamped to yield surface (controls when snow breaks apart vs. compresses)
  - User parameters: critical compression/stretch ratios, hardening coefficient
- **Grid-based semi-implicit integration:** linear system conditioning independent of particle count
- **Automatic self-collision:** grid naturally prevents interpenetration
- **Automatic fracture:** particles separate when yield criterion exceeded — no crack tracking needed
- Complex character interactions demonstrated (Frozen production pipeline)

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Comparison to real snow behavior (qualitative) |
| Metrics | Visual plausibility, parameter sensitivity |
| Runtime | Not real-time (production quality), but practical for film |
| Failure cases | Extreme parameter choices can produce unrealistic behavior |

## Likely Weaknesses

- Qualitative validation only — no quantitative comparison to measured snow mechanics
- Grid resolution limits detail (common MPM limitation)
- Particle-grid transfer introduces numerical diffusion
- Semi-implicit integration may struggle with very stiff snow configurations
- 2013 implementation — modern GPU MPM (e.g., Taichi-based) is significantly faster

## Verdict

Landmark paper that established [[MPM]] as a first-class simulation method in graphics. The insight that snow's solid-fluid duality maps perfectly onto MPM's hybrid nature was a breakthrough. Spawned an enormous body of follow-up work: granular MPM (Daviet & Bertails-Descoubes 2016), CD-MPM for fracture, phase-field MPM, and the entire Taichi MPM ecosystem. Essential reading for anyone working with MPM or multi-phase materials.

## Related Papers

- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]] — extends MPM with continuum damage for fracture
- [[20_Research/Papers/Simulation/A semi-implicit material point method for granular materials|Semi-implicit MPM for Granular]] — Drucker-Prager yield for granular flow
- [[20_Research/Papers/Simulation/CK-MPM|CK-MPM]] — compact kernel improvement to MPM stability
