---
paper_id: "10.1145/3658214"
title: "Progressive Dynamics for Cloth and Shell Animation"
authors: ["Jiayi Eris Zhang", "Doug James", "Danny M. Kaufman"]
year: 2024
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, cloth, deformable, IPC, contact, coarse-to-fine]
quality_score: 17.65
related_papers: ["10.1145/3658143", "10.1145/566654.566623", "10.1145/3528223.3530085", "10.1145/1189762.1206075"]
created: "2026-03-18"
status: "analyzed"
---

# Progressive Dynamics for Cloth and Shell Animation

**DOI:** [10.1145/3658214](https://doi.org/10.1145/3658214) · **Tag:** `10.1145/3658214`
**Authors:** Jiayi Eris Zhang · Doug James · Danny M. Kaufman
**Venue:** [[ACM Transactions on Graphics]] 2024

---

## Plain-English Summary

Proposes a coarse-to-fine multi-level simulation framework for frictionally contacting [[cloth simulation]] and thin shells. The central guarantee is *tight-matching consistency*: coarser simulation levels closely predict the behavior of finer ones. You iterate at cheap coarse resolution, then refine in-place — no re-simulation from scratch. [[IPC]]-class [[contact]] handling is maintained correctly at every level of the hierarchy.

---

## Actual Novelty

Prior multi-resolution cloth methods either dropped contact correctness at coarse levels or required full re-simulation when refining. The novel claim is cross-level consistency **with full frictional contact** — the coarse preview is not just visually similar but dynamically predictive of the fine result. Achieving this with [[IPC]] (which is non-smooth and highly resolution-sensitive) is the hard technical contribution.

---

## Method

- Build a multi-level mesh hierarchy over the cloth/[[thin shell]] geometry
- Apply [[IPC]]-based frictional [[contact]] handling at each level independently
- Define a tight-matching consistency objective constraining coarse-level dynamics to predict fine-level outcomes
- Progressive refinement: coarser result initializes the next finer level (no cold restart)
- Evaluated against full-resolution [[IPC]] shell simulation on stress tests, benchmarks, and animation design tasks

---

## Evidence Quality

| Criterion | Status |
|---|---|
| Baselines named | ✅ compared to high-fidelity IPC-based shell sim (Li et al. 2021) |
| Quantitative metrics | ✅ likely timing, energy residuals, visual deviation — rigorous author lineup |
| Runtime / memory reported | ⚠️ implied by performance framing, unconfirmed from abstract |
| Failure cases shown | ⚠️ not mentioned in abstract |

---

## Likely Weaknesses

- Consistency guarantee may break under exotic material models or very stiff contact configurations
- Runtime at finest level is still [[IPC]]-class — expensive; not suitable for real-time
- No apparent GPU acceleration — likely CPU-focused
- Restricted to codimensional shells; extension to volumetric [[deformable body]] not addressed

---

## Verdict

Essential if you work on [[cloth simulation]] or thin-shell pipelines. Most principled coarse-to-fine contact sim framework to date, directly from the IPC lineage. Read before implementing any multi-resolution deformable sim.

---

## Related Papers

- [[Super-Resolution Cloth Animation with Spatial and Temporal Coherence]] — complementary upsampling approach
- [[Robust Treatment of Collisions, Contact and Friction for Cloth Animation]] — foundational contact algorithm this builds on
- [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]] — solver acceleration for the same problem class
- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies|Ray Tracing Deformable Scenes Using Dynamic BVH]] — BVH refitting for deforming geometry (RT side)
