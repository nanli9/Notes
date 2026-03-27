---
paper_id: "10.1145/3687934"
title: "3D Gaussian Ray Tracing: Fast Tracing of Particle Scenes"
authors: ["Nicolas Moenne-Loccoz", "Ashkan Mirzaei", "Or Perel", "Riccardo de Lutio", "Janick Martinez Esturo", "Gavriel State", "Sanja Fidler", "Nicholas Sharp", "Zan Gojcic"]
year: 2024
venue: "ACM Transactions on Graphics"
domain: "Vulkan_Ray_Tracing"
tags: [paper-notes, Vulkan-Ray-Tracing, BVH, gaussian-splatting, hardware-ray-tracing, semi-transparent]
quality_score: 16.15
related_papers: ["10.1145/1189762.1206075", "10.1145/3658214"]
created: "2026-03-18"
status: "analyzed"
---

# 3D Gaussian Ray Tracing: Fast Tracing of Particle Scenes

**DOI:** [10.1145/3687934](https://doi.org/10.1145/3687934) · **Tag:** `10.1145/3687934`
**Authors:** Moenne-Loccoz, Mirzaei, Perel, de Lutio et al. (NVIDIA)
**Venue:** [[ACM Transactions on Graphics]] 2024

---

## Plain-English Summary

Replaces rasterization in [[3D Gaussian Splatting]] with hardware [[BVH]] ray tracing. Each Gaussian particle is wrapped in a bounding mesh so it can exploit fast ray-triangle intersection hardware; batches of intersected Gaussians are shaded in depth order. Unlocks secondary effects (shadows, reflections), distorted cameras, and stochastic sampling at cost competitive with rasterization.

---

## Actual Novelty

Semi-transparent particles need depth-ordered compositing, which conflicts with ray tracing's incoherent traversal. The bounding-mesh wrapping + depth-sorted batch shading solves this practically. Also proposes generalized kernel functions that significantly reduce per-ray particle hit counts. Prior [[3D Gaussian Splatting]] work always used rasterization; this is the first to make hardware [[BVH|RT]] viable for Gaussian scenes.

---

## Method

- Build [[BVH]] over all Gaussian particles
- Wrap each Gaussian in a lightweight bounding mesh → fast ray-triangle intersection on hardware
- On ray hit: collect all intersected Gaussians per ray, sort by depth, shade in compositing order
- Batch shading amortizes depth-sort cost across ray coherence groups
- Generalized kernel functions reduce average hit count per ray

---

## Evidence Quality

| Criterion | Status |
|---|---|
| Baselines named | ✅ rasterization-based 3DGS directly compared |
| Quantitative metrics | ✅ speed and accuracy benchmarks (NVIDIA paper) |
| Runtime / memory reported | ✅ central to paper |
| Failure cases shown | ⚠️ not mentioned; dense overlapping Gaussians may stress depth-sort |

---

## Likely Weaknesses

- Bounding mesh wrapping introduces geometric approximation; very tight Gaussians may yield poor [[BVH]] efficiency
- Depth-sorted batch shading is approximate — not a strict per-pixel sort; blending artifacts possible in dense scenes
- Tied to NVIDIA RT hardware ([[Vulkan]] / DXR); non-RTX portability limited
- No evaluation on highly dynamic scenes where [[BVH]] rebuild cost dominates

---

## Verdict

Read for the [[BVH]] construction and semi-transparent particle traversal strategy — directly applicable to [[Vulkan]] RT pipelines handling volumetric or particle-based representations.

---

## Related Papers

- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies|Ray Tracing Deformable Scenes Using Dynamic BVH]] — foundational BVH refit paper, explains the traversal design space
- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics for Cloth and Shell Animation]] — deformable geometry + BVH intersection (simulation side)
