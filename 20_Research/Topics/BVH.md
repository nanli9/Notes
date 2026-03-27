---
title: "BVH — Bounding Volume Hierarchy"
type: topic
tags: [topic, Vulkan-Ray-Tracing, acceleration-structure, spatial-indexing]
---

# BVH — Bounding Volume Hierarchy

A tree-based spatial acceleration structure where each node stores a bounding volume (usually AABB) that encloses all geometry in its subtree. Used for fast ray-object intersection and collision detection.

**Build strategies:**
- [[SAH]] (Surface Area Heuristic) — best quality, used for ray tracing
- Median split, binning — faster build, lower quality

**For ray tracing (Vulkan / DXR):**
- Top-Level AS (TLAS) — scene instances
- Bottom-Level AS (BLAS) — individual mesh geometry
- **BLAS refit** — update bounding volumes without changing topology (O(n), for [[deformable body|deforming meshes]])
- **BLAS rebuild** — full reconstruction (better quality, higher cost)

**For deformable geometry:**
- Topology-fixed refitting handles small-to-medium deformations efficiently
- Large deformations degrade tree quality — periodic rebuild needed
- Foundational paper: [[Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]]

## Papers
- [[Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]] — dynamic BVH for deformable scenes
- [[3D Gaussian Ray Tracing Fast Tracing of Particle Scenes]] — BVH for Gaussian particle scenes
- [[20_Research/Papers/Vulkan Ray Tracing/Tessellation-free Displacement Mapping for Ray Tracing|Tessellation-free Displacement Mapping for Ray Tracing]]
- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing on Programmable Graphics Hardware|Ray Tracing on Programmable Graphics Hardware]]
- [[20_Research/Papers/Vulkan Ray Tracing/Fused BVH to Ray Trace Level of Detail Meshes|Fused BVH to Ray Trace Level of Detail Meshes]]
