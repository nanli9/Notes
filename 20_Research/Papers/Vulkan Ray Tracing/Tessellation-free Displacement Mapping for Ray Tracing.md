---
paper_id: "10.1145/3478513.3480535"
title: "Tessellation-free Displacement Mapping for Ray Tracing"
authors: ["Theo Thonat", "Francois Beaune", "Xin Sun", "Nathan Carr", "Tamy Boubekeur"]
year: 2021
venue: "ACM Transactions on Graphics"
domain: "Vulkan Ray Tracing"
tags: [paper-notes, Vulkan-Ray-Tracing, Rendering, displacement-mapping, BVH, memory-efficient]
quality_score: 11.65
related_papers: ["10.1145/3618359", "10.1145/1189762.1206075"]
created: "2026-03-19"
status: "analyzed"
---

# Tessellation-free Displacement Mapping for Ray Tracing

**DOI:** [10.1145/3478513.3480535](https://doi.org/10.1145/3478513.3480535) · **Tag:** `10.1145/3478513.3480535`

## Plain-English Summary

Displacement mapping adds geometric detail to surfaces via a 2D heightfield texture, but ray tracing displaced geometry traditionally requires pre-tessellating the mesh to displacement resolution and building an acceleration structure over the result — consuming enormous memory. This paper decouples the displacement from the base mesh by mapping a displacement-specific [[BVH]] directly onto the surface. Result: low memory footprint, fast high-resolution displacement rendering, and interactive displacement editing during physically-based rendering.

## Actual Novelty

Key insight: instead of tessellating the mesh (which multiplies triangle count and memory), build a per-surface acceleration structure that operates in the displacement map's UV space. Ray traversal is remapped into this 2D displacement space, avoiding the need to ever materialize the tessellated geometry. This decoupling means tiling the same displacement map multiple times costs almost nothing — a scenario that normally saturates GPU memory.

## Method

- Decouple displacement from base domain — no pre-tessellation
- Map a displacement-specific acceleration structure directly onto each mesh surface
- Ray intersection: transform ray into UV space, traverse displacement BVH in 2D
- Shading and visibility computed independently — acceleration structure only handles displacement intersection
- Low memory: store only base mesh + displacement texture + lightweight per-surface BVH
- Interactive editing: changing displacement map only requires local BVH rebuild

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to pre-tessellated ray tracing (standard approach) |
| Metrics | Memory footprint, rendering time, visual quality |
| Runtime | Interactive displacement editing with full path tracing |
| Failure cases | Extreme displacement ratios or highly irregular base meshes may degrade |

## Likely Weaknesses

- Per-surface BVH traversal adds indirection — may not map perfectly to hardware RT units ([[Vulkan]] RT pipeline expects triangle BVH)
- Silhouette quality depends on base mesh resolution (displacement only adds detail within triangles)
- Self-shadowing from displacement may require careful handling
- UV seam artifacts possible at displacement boundaries

## Verdict

Elegant memory-efficient solution for a real production pain point. Displacement mapping is ubiquitous in film/game assets, and the memory explosion when ray tracing displaced geometry is a genuine bottleneck. The tessellation-free approach is particularly relevant for [[Vulkan]] hardware RT where BLAS memory is a constraint. Good complement to [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies|dynamic BVH]] work.

## Related Papers

- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies|Dynamic BVH for Deformable Scenes]] — related BVH optimization for RT
- [[20_Research/Papers/Vulkan Ray Tracing/Locally-Adaptive Level-of-Detail for Hardware-Accelerated Ray Tracing|Locally-Adaptive LOD for RT]] — complementary LOD approach
