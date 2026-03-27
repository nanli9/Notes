---
paper_id: "10.1145/3550082.3564211"
title: "Fused BVH to Ray Trace Level of Detail Meshes"
authors: ["Paritosh Kulkarni", "Sho Ikeda", "Takahiro Harada"]
year: 2022
venue: "SIGGRAPH Asia 2022 Posters"
domain: "Vulkan Ray Tracing"
tags: [paper-notes, Vulkan-Ray-Tracing, BVH, LOD, mesh, acceleration-structure]
quality_score: 10.15
related_papers: ["10.1145/3618359", "10.1145/3478513.3480535"]
created: "2026-03-22"
status: "analyzed"
---

# Fused BVH to Ray Trace Level of Detail Meshes

**DOI/arXiv:** [10.1145/3550082.3564211](https://doi.org/10.1145/3550082.3564211) · **Tag:** `10.1145/3550082.3564211`

## Plain-English Summary
This poster presents a method for fusing multiple [[BVH]] acceleration structures — one per level-of-detail (LOD) mesh — into a single unified BVH. The goal is to enable efficient hardware [[ray tracing]] of scenes where objects are represented at multiple LOD levels, avoiding the overhead of maintaining separate acceleration structures or switching between them during traversal.

## Actual Novelty
Fusing per-LOD BVHs into a single structure that can be traversed in one pass on RT hardware. This avoids the typical approach of selecting a single LOD per object before tracing, which can produce artifacts at LOD boundaries. *Note: only abstract/poster available — detailed algorithmic contribution is unclear.*

## Method
*(Limited to what can be inferred from title and venue — no abstract provided)*
- Construct individual BVHs for each LOD mesh
- Fuse these into a single [[BVH]] that encodes LOD transitions
- Ray traversal selects appropriate LOD during BVH descent based on ray footprint or distance

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Unknown (poster) |
| Metrics | Unknown |
| Runtime | Unknown |
| Failure cases | Unknown |

**Caveat:** This is a SIGGRAPH Asia poster — the full evaluation is not available from metadata alone. Claims are limited accordingly.

## Likely Weaknesses
- **Poster-only:** no full paper with detailed evaluation or comparisons
- **BVH quality:** fusing heterogeneous LOD BVHs may produce suboptimal [[SAH]] quality compared to building a single BVH over the active LOD set
- **Hardware compatibility:** unclear if the fused structure is compatible with hardware RT units (TLAS/BLAS) without custom traversal
- **Dynamic scenes:** LOD selection at trace time adds per-ray branching cost

## Verdict
Interesting idea at the intersection of [[BVH]] construction and LOD management for hardware RT. The concept of fusing LOD BVHs is relevant to modern [[Vulkan]] RT pipelines where complex scenes use aggressive LOD. However, as a poster without full evaluation, the practical impact is hard to assess. See [[20_Research/Papers/Vulkan Ray Tracing/Locally-Adaptive Level-of-Detail for Hardware-Accelerated Ray Tracing|Locally-Adaptive LOD (2023)]] for a full TOG treatment of related ideas.

## Related Papers
- [[20_Research/Papers/Vulkan Ray Tracing/Locally-Adaptive Level-of-Detail for Hardware-Accelerated Ray Tracing|Locally-Adaptive LOD for RT (2023)]] — full paper on LOD + hardware RT
- [[20_Research/Papers/Vulkan Ray Tracing/Tessellation-free Displacement Mapping for Ray Tracing|Tessellation-free Displacement (2021)]] — related BVH-level geometry management
