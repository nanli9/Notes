---
title: "Vulkan Ray Tracing"
type: topic
tags: [topic, Vulkan-Ray-Tracing, GPU, hardware-ray-tracing, real-time]
---

# Vulkan Ray Tracing

Vulkan extensions (`VK_KHR_ray_tracing_pipeline`, `VK_KHR_acceleration_structure`) that expose hardware ray tracing on modern GPUs (NVIDIA RTX, AMD RDNA2+, Intel Arc).

**Pipeline stages:**
- Ray generation shader — launches rays
- Intersection shader — custom geometry intersection
- Any-hit shader — semi-transparent surfaces
- Closest-hit shader — shading on closest intersection
- Miss shader — ray escapes scene

**Acceleration structures:**
- TLAS (Top-Level AS) — instances in the scene
- BLAS (Bottom-Level AS) — individual mesh geometry
  - **Refit** — update bounding volumes, keep topology (fast, for [[deformable body|deforming meshes]])
  - **Rebuild** — full reconstruction (slower, better quality)

**Key algorithms:**
- [[BVH]] traversal in hardware
- ReSTIR — reservoir-based spatiotemporal importance resampling (shadows, GI)
- SVGF — spatiotemporal variance-guided filtering (denoising)
- Hybrid rendering — rasterize primary, raytrace secondary effects

## Papers
- [[Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies]] — foundational BLAS refit concept
- [[3D Gaussian Ray Tracing Fast Tracing of Particle Scenes]] — hardware RT for particle/volumetric scenes
- [[20_Research/Papers/Vulkan Ray Tracing/Tessellation-free Displacement Mapping for Ray Tracing|Tessellation-free Displacement Mapping for Ray Tracing]]
- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing on Programmable Graphics Hardware|Ray Tracing on Programmable Graphics Hardware]]
- [[20_Research/Papers/Vulkan Ray Tracing/Fused BVH to Ray Trace Level of Detail Meshes|Fused BVH to Ray Trace Level of Detail Meshes]]
