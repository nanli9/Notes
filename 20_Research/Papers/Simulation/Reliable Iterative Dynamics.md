---
paper_id: "10.1145/3734518"
title: "Reliable Iterative Dynamics: A Versatile Method for Fast and Robust Simulation"
authors: ["Jia-Ming Lu", "Shi-Min Hu"]
year: 2025
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, solver, stiff-materials, FEM, MPM, SPH, IPC, real-time]
quality_score: 24.9
related_papers: ["10.1145/3658184", "10.1145/3731182", "10.1145/3658214"]
created: "2026-03-20"
status: "analyzed"
---

# Reliable Iterative Dynamics

**DOI:** [10.1145/3734518](https://doi.org/10.1145/3734518) · **Tag:** `10.1145/3734518`

## Plain-English Summary

A new iterative solver called "Reliable Iterative Dynamics" (RID) that handles extremely stiff materials without needing tiny time steps or many iterations. The key promise: every intermediate iteration produces a visually plausible configuration — no explosive or collapsed states during convergence. Works across [[FEM]], [[MPM]], [[SPH]], and [[IPC]] formulations, making it a drop-in replacement for existing solvers in diverse simulation pipelines.

## Actual Novelty

A dual descent framework that provides theoretical guarantees for "visual reliability" at each iteration. Unlike projective dynamics (which only handles constraint-based formulations) or standard Newton methods (which can produce garbage intermediate states), RID ensures monotonic improvement in visual quality while converging. The core innovation circumvents the fundamental trade-off between stiffness handling and iteration count.

## Method

- **Dual descent framework:** two coupled optimization objectives that together guarantee visually reliable intermediate states
- **No small timestep requirement:** handles materials from soft to infinitely rigid with large time steps
- **Minimal iterations:** produces acceptable results with very few iterations per frame
- **Unified formulation:** works with:
  - [[FEM]] for elastic bodies
  - [[MPM]] for granular/plastic materials
  - [[SPH]] for fluids
  - [[IPC]] for [[contact]] handling
- Seamless integration with existing simulation codebases

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared against PBD/XPBD, projective dynamics, Newton's method |
| Metrics | Convergence rate, visual reliability per iteration, stiffness range |
| Runtime | Real-time for stiff materials that break other solvers |
| Failure cases | Not discussed in abstract — need full paper |

## Likely Weaknesses

- TOG 2025 — very recent, no independent reproduction yet
- "Visual reliability" guarantee is a weaker claim than physical accuracy
- Performance overhead of dual descent vs. single descent unclear
- May trade accuracy for reliability in extreme cases
- Abstract doesn't detail the computational cost per iteration

## Verdict

Potentially significant advance for real-time physics. The key selling point is practical: every iteration looks reasonable, which matters enormously for interactive applications where you can't afford to wait for full convergence. The breadth of supported formulations (FEM + MPM + SPH + IPC) is impressive and suggests the method is genuinely fundamental rather than formulation-specific. The 2025 TOG placement and the Tsinghua affiliation (Shi-Min Hu's group) add credibility.

## Related Papers

- [[20_Research/Papers/Simulation/Simplicits|Simplicits]] — alternative geometry-agnostic simulation approach
- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics]] — related multi-resolution solver for cloth
- [[20_Research/Papers/Simulation/High-performance CPU Cloth Simulation Using Domain-decomposed Projective Dynamics|CPU Cloth via Domain-decomposed PD]] — projective dynamics variant this could replace
