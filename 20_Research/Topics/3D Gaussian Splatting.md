---
title: "3D Gaussian Splatting"
type: topic
tags: [topic, Neural-Rendering, gaussian-splatting, real-time, novel-view-synthesis]
---

# 3D Gaussian Splatting (3DGS)

Neural scene representation using 3D Gaussian primitives optimized from multi-view images. Each Gaussian has position, covariance (shape/orientation), opacity, and spherical harmonic color coefficients. Rasterized via tile-based alpha compositing for real-time novel-view synthesis.

**Original paper:** Kerbl et al., SIGGRAPH 2023 — `10.1145/3592433`

**Rendering pipeline (standard):**
- Project Gaussians to 2D screen splats
- Sort by depth per tile
- Alpha-composite front-to-back

**Limitations of rasterization:**
- No secondary effects (shadows, reflections)
- Popping artifacts during view rotation
- Assumes perspective-correct projection

**Ray tracing alternative:**
- [[3D Gaussian Ray Tracing Fast Tracing of Particle Scenes]] — hardware [[BVH]] RT for Gaussians; enables secondary effects at rasterization cost

**Variants relevant to simulation:**
- 4D Gaussian Splatting — dynamic scenes
- Mesh-in-the-Loop (MILo) — extracts simulation-ready meshes from Gaussians

## Papers
- [[20_Research/Papers/Vulkan Ray Tracing/3D Gaussian Ray Tracing Fast Tracing of Particle Scenes|3D Gaussian Ray Tracing Fast Tracing of Particle Scenes]]
- [[20_Research/Papers/Simulation/Simplicits|Simplicits]]
- [[20_Research/Papers/Neural Rendering/SGAD-SLAM Splatting Gaussians at Adjusted Depth for Better Radiance Fields in RGBD SLAM|SGAD-SLAM Splatting Gaussians at Adjusted Depth for Better Radiance Fields in RGBD SLAM]]
