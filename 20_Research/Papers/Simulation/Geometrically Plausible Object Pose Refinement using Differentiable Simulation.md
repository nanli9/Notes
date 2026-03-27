---
paper_id: "arxiv:2603.20992"
title: "Geometrically Plausible Object Pose Refinement using Differentiable Simulation"
authors: ["Anil Zeybek", "Rhys Newbury", "Snehal Dikhale", "Nawid Jamali", "Soshi Iba", "Akansel Cosgun"]
year: 2026
venue: "arXiv"
domain: "Simulation"
tags: [paper-notes, Simulation, differentiable-simulation, pose-estimation, robotics, contact]
quality_score: 12.55
related_papers: ["10.1145/3657648", "10.1145/3386569.3392425"]
created: "2026-03-24"
status: "analyzed"
---

# Geometrically Plausible Object Pose Refinement using Differentiable Simulation

**DOI/arXiv:** [arxiv:2603.20992](http://arxiv.org/abs/2603.20992v1) · **Tag:** `arxiv:2603.20992`

## Plain-English Summary

Pose estimation methods often produce results where objects clip through the robot hand or float above surfaces. This paper uses [[differentiable simulation]] combined with differentiable rendering and tactile sensing to refine poses so they are physically consistent — objects don't interpenetrate the hand and rest properly on support surfaces. The optimization jointly minimizes visual/tactile error and physics-based penetration penalties.

## Actual Novelty

The core contribution is combining three differentiable modalities (physics sim, rendering, tactile) into a single pose refinement loop. The physics sim provides gradients that push the object out of interpenetration, while rendering and tactile losses keep it faithful to sensor observations. This is not a new simulation method per se — it's an application of differentiable sim to a robotics perception problem.

## Method

- Start with an initial 6-DoF pose estimate from any off-the-shelf estimator
- Differentiable physics simulation computes [[contact]] forces and penetration gradients between object mesh and robot hand mesh
- Differentiable rendering provides image-space gradients via silhouette/depth matching
- Tactile sensing adds local surface contact constraints
- Joint optimization via gradient descent refines the pose to satisfy all three objectives

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | ICP-based refinement only — no comparison to other differentiable refinement methods |
| Metrics | Intersection volume error, translation error, orientation error |
| Runtime | Not reported |
| Failure cases | Not discussed |

## Likely Weaknesses

- **Simulation-only evaluation** — no real-robot experiments, limiting practical claims
- **Narrow baselines** — ICP is a weak baseline; should compare against learning-based refiners (e.g., DeepIM, CosyPose)
- **No runtime analysis** — differentiable sim + rendering in the loop could be slow for real-time manipulation
- **Mesh quality dependency** — relies on having accurate meshes for both object and hand

## Verdict

A reasonable proof-of-concept showing differentiable physics can improve pose plausibility. The idea is sound but the evaluation is preliminary — simulated-only, weak baselines, no runtime reporting. More of a workshop-level contribution than a full paper in its current form. Relevant to anyone combining differentiable simulation with perception, but not directly advancing simulation methods themselves.

## Related Papers

- [[20_Research/Papers/Simulation/Differentiable Solver for Time-Dependent Deformation with Contact|Differentiable Solver for Deformation with Contact]] — differentiable sim with [[IPC]]-based [[contact]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|IPC]] — foundational contact method
