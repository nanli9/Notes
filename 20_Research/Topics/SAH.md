---
title: "SAH — Surface Area Heuristic"
type: topic
tags: [topic, Vulkan-Ray-Tracing, BVH, spatial-indexing]
---

# SAH — Surface Area Heuristic

A cost model used to guide [[BVH]] and kd-tree construction. Estimates the expected ray traversal cost of a split by assuming rays are uniformly distributed. The cost of a node is proportional to the probability a ray hits it (estimated by its surface area relative to the parent) times the cost of traversing its children.

**Why it matters:**
- SAH-built BVHs match or outperform kd-trees for most ray tracing workloads
- Shown in [[Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]] (Wald et al. 2007)
- Now the standard build strategy in Vulkan / DXR hardware acceleration structure construction

**Build variants:**
- Full SAH — evaluate all split positions (expensive, best quality)
- Binned SAH — discretize split candidates (fast, near-optimal)
- HLBVH — sort by Morton code first, then SAH within bins (GPU-friendly)

## Papers
- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies|Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]]
- [[20_Research/Papers/Vulkan Ray Tracing/Fused BVH to Ray Trace Level of Detail Meshes|Fused BVH to Ray Trace Level of Detail Meshes]]
