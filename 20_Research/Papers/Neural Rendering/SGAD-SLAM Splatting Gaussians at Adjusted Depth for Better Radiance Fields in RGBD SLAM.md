---
paper_id: "arxiv:2603.21055"
title: "SGAD-SLAM: Splatting Gaussians at Adjusted Depth for Better Radiance Fields in RGBD SLAM"
authors: ["Pengchong Hu", "Zhizhong Han"]
year: 2026
venue: "arXiv"
domain: "Neural Rendering"
tags: [paper-notes, Neural-Rendering, 3D Gaussian Splatting, SLAM, depth-adjustment, radiance-fields]
quality_score: 9.80
related_papers: []
created: "2026-03-24"
status: "analyzed"
---

# SGAD-SLAM: Splatting Gaussians at Adjusted Depth for Better Radiance Fields in RGBD SLAM

**DOI/arXiv:** [arxiv:2603.21055](http://arxiv.org/abs/2603.21055v1) · **Tag:** `arxiv:2603.21055`

## Plain-English Summary

Current [[3D Gaussian Splatting]]-based SLAM methods either let Gaussians move too freely (slow convergence) or pin them rigidly to views (limited quality). This paper uses pixel-aligned Gaussians but allows each one to slide along its ray to find the optimal depth, balancing flexibility and convergence speed. Tracking is accelerated by modeling per-pixel depth distributions as Gaussians for fast frame-to-scene alignment.

## Actual Novelty

The depth-adjustment idea is the main contribution: pixel-aligned Gaussians are constrained to their ray but free along depth. This is a clean middle ground between fully free 3DGS (too many DoF) and view-tied approaches (too rigid). The Gaussian depth distribution for tracking is a secondary but practical contribution.

## Method

- Pixel-aligned Gaussians initialized from RGBD frames
- Each Gaussian parameterized with a learnable depth offset along its ray
- Simplified Gaussian attributes (fewer parameters) for better scalability
- Tracking: model depth distribution per pixel as 1D Gaussian, align frames via distribution matching
- Mapping: optimize Gaussian positions and attributes with photometric + depth loss

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Compared against "latest methods" on standard benchmarks (specific names in paper) |
| Metrics | Rendering quality (PSNR, SSIM, LPIPS), camera tracking (ATE), runtime, storage |
| Runtime | Reported — claims advantages in speed |
| Failure cases | Not discussed |

## Likely Weaknesses

- **Incremental improvement** — the ray-constrained depth adjustment is a sensible but relatively simple modification
- **RGBD dependency** — requires depth input; less applicable to monocular settings
- **Only abstract available for deep analysis** — full method details need the paper
- **Failure modes unclear** — how does it handle depth noise, reflective surfaces, transparent objects?

## Verdict

A practical improvement to 3DGS-based SLAM with a clean design choice (ray-constrained depth freedom). Claims improvements in rendering, tracking, speed, and storage simultaneously, which is a strong result if it holds up. Worth watching for anyone interested in real-time [[3D Gaussian Splatting]] applications. Project page with code available.

## Related Papers

- [[3D Gaussian Splatting]] — foundational technique
