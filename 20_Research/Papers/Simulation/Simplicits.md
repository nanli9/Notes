---
paper_id: "10.1145/3658184"
title: "Simplicits: Mesh-Free, Geometry-Agnostic Elastic Simulation"
authors: ["Vismay Modi", "Nicholas Sharp", "Or Perel", "Shinjiro Sueda", "David I. W. Levin"]
year: 2024
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, mesh-free, neural, reduced-order, elastic, geometry-agnostic]
quality_score: 23.4
related_papers: ["10.1145/3734518", "10.1145/2019627.2019640", "10.1145/1531326.1531359"]
created: "2026-03-20"
status: "analyzed"
---

# Simplicits: Mesh-Free, Geometry-Agnostic Elastic Simulation

**DOI:** [10.1145/3658184](https://doi.org/10.1145/3658184) · **Tag:** `10.1145/3658184`

## Plain-English Summary

A simulation method that works on *any* 3D representation — meshes, point clouds, SDFs, radiance fields, [[3D Gaussian Splatting|Gaussian splats]], CT scans — without requiring a traditional mesh. The key idea: fit a small neural network per object that encodes spatially varying deformation weights (a reduced basis), then simulate in this low-dimensional space. Training uses Monte Carlo sampling of elastic energy over the object's volume, requiring only an occupancy function (does a point lie inside the object?).

## Actual Novelty

The observation that every geometric representation can be reduced to a common interface — an occupancy query — and that a reduced deformation basis can be learned atop this interface via stochastic energy minimization. This decouples the simulator from the representation entirely. Prior reduced-order methods required explicit meshes for basis construction; Simplicits needs only point samples.

## Method

- **Common interface:** any representation → occupancy function $\mathbf{x} \mapsto \{0, 1\}$
- **Reduced basis:** small implicit neural network $\mathbf{w}(\mathbf{x})$ maps spatial position to deformation weights
- **Training:** random perturbations + Monte Carlo sampling of elastic energy → learn physically meaningful deformation modes
- **Volume estimation:** new per-particle volume estimate allows spatially/temporally varying particle masses at fixed density
- **Runtime:** simulate in reduced weight space (low-dimensional ODE), sample deformations back to original representation
- **Supports:** various material energies, [[contact]] models, [[time integration]] schemes
- **Tested on:** SDFs, point clouds, neural primitives, tomography, radiance fields, Gaussian splats, surface/volume meshes

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to mesh-based FEM on equivalent geometries |
| Metrics | Visual quality, deformation accuracy, generality across representations |
| Runtime | Fast (reduced basis), but training per-object is offline |
| Failure cases | Extremely detailed deformations may exceed basis expressiveness |

## Likely Weaknesses

- Per-object neural network training required (offline precomputation)
- Reduced basis limits the space of achievable deformations — fine details lost
- Monte Carlo energy evaluation introduces sampling noise
- Occupancy-only interface loses surface normal information that meshes provide
- Neural network inference adds overhead vs. direct matrix operations in classical reduced FEM

## Verdict

Elegant unifying framework that reflects the current proliferation of 3D representations (NeRF, Gaussian splats, implicit surfaces, etc.). The practical value is clear: you can simulate any object without first converting to a simulation mesh. The reduced basis approach is well-motivated — similar to [[20_Research/Papers/Simulation/Fast Simulation of Skeleton-Driven Deformable Body Characters|skeleton-driven deformable]] work but without requiring a skeleton. The NVIDIA + University of Toronto team and SIGGRAPH 2024 placement indicate strong work.

## Related Papers

- [[20_Research/Papers/Simulation/Reliable Iterative Dynamics|Reliable Iterative Dynamics]] — complementary solver that could be used with this representation
- [[20_Research/Papers/Simulation/Fast Simulation of Skeleton-Driven Deformable Body Characters|Skeleton-Driven Deformable Sim]] — related reduced-order approach (skeleton-specific)
- [[20_Research/Papers/Simulation/Deformable Object Animation Using Reduced Optimal Control|Reduced Optimal Control]] — earlier reduced-order deformable method
