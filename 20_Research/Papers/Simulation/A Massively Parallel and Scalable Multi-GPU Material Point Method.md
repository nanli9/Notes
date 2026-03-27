---
paper_id: "10.1145/3386569.3392442"
title: "A Massively Parallel and Scalable Multi-GPU Material Point Method"
authors: ["Xinlei Wang", "Yuxing Qiu", "Stuart R. Slattery", "Yu Fang", "Minchen Li", "Song-Chun Zhu", "Yixin Zhu", "Min Tang", "Dinesh Manocha", "Chenfanfu Jiang"]
year: 2020
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, MPM, GPU-simulation, multi-GPU, parallel-computing]
quality_score: 20.90
related_papers: ["10.1145/2461912.2461948", "10.1145/3272127.3275044", "10.1145/3731155"]
created: "2026-03-21"
status: "analyzed"
---

# A Massively Parallel and Scalable Multi-GPU Material Point Method

**DOI/arXiv:** [10.1145/3386569.3392442](https://doi.org/10.1145/3386569.3392442) · **Tag:** `10.1145/3386569.3392442`

## Plain-English Summary

This paper presents a high-performance [[GPU simulation]] system for [[MPM]] that scales across multiple GPUs. Three architectural innovations make this possible: (1) an Array-of-Structs-of-Array (AoSoA) particle data structure for coalesced memory access, (2) a fused Grid-to-Particles-to-Grid (G2P2G) kernel that eliminates intermediate storage and reduces kernel launches, and (3) sparse grid algorithms optimized for GPU shared memory. The result is 100× speedup over optimized CPU MPM and near-perfect multi-GPU scaling.

## Actual Novelty

The G2P2G kernel fusion is the standout contribution. Traditional [[MPM]] requires separate G2P and P2G passes with particle state stored between them. By fusing these into a single kernel, the method eliminates global memory writes for particle velocities and deformation gradients mid-step, dramatically reducing memory bandwidth. The AoSoA layout ensures that particles within the same cell are contiguous in memory, enabling coalesced access without complex sorting. Together these eliminate the need for atomic operations on the GPU memory hierarchy.

## Method

1. **AoSoA data structure**: particles grouped by grid cell; each block stores a struct of arrays for one cell's particles. Eliminates atomic operations during P2G scatter.
2. **G2P2G fusion**: grid-to-particle interpolation and particle-to-grid transfer combined in one kernel. Particle state (velocity, deformation gradient) updated in registers, never written to global memory.
3. **Sparse grid via hashing**: active grid blocks tracked with spatial hashing; shared memory used for block-local grid nodes during transfer operations.
4. **Multi-GPU partitioning**: domain decomposition across GPUs with halo exchange for boundary grid nodes. Near-perfect weak and strong scaling demonstrated with 4–8 GPUs.
5. Supports elastoplastic, granular, and fluid constitutive models.

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Direct comparison against Fang et al. 2019 (CPU MPM) and Hu et al. 2019 (Taichi GPU MPM) |
| Metrics | Per-timestep wall-clock time, speedup ratios, scaling efficiency |
| Runtime | 100M particles on 1024³ grid: <4 min/frame (4 GPUs), <1 min/frame (8 GPUs) |
| Failure cases | Implicit solve scaling not demonstrated; all examples use explicit integration |

## Likely Weaknesses

- **Explicit integration only**: all benchmarks use explicit MPM; implicit solvers (needed for stiff materials) would have very different scaling characteristics due to global linear solves
- **Domain decomposition overhead**: halo exchange cost grows with surface-to-volume ratio of partitions; not analyzed for highly irregular particle distributions
- **Memory**: AoSoA layout wastes memory when cells have varying particle counts (padding to max PPC per block)
- **Limited constitutive models tested**: no cloth, thin shell, or complex fracture examples
- **Hardware specific**: optimizations tailored to NVIDIA Quadro/Tesla; portability to other GPU architectures unclear

## Verdict

A systems-level tour de force that establishes the engineering blueprint for large-scale GPU [[MPM]]. The G2P2G fusion is an elegant idea that others have since adopted. The 100× speedup over CPU and near-linear multi-GPU scaling are convincing. However, the restriction to explicit integration limits applicability for stiff materials and production cloth/shell simulation. The follow-up work (MPM Lite, CK-MPM) addresses some of these gaps. Essential reference for anyone implementing [[GPU simulation]] with particle methods.

## Related Papers

- [[20_Research/Papers/Simulation/A Material Point Method for Snow Simulation|MPM for Snow]] — foundational MPM paper this builds upon
- [[20_Research/Papers/Simulation/A Material Point Method for Thin Shells with Frictional Contact|MPM Thin Shells]] — extends MPM to shells (CPU-focused)
- [[20_Research/Papers/Simulation/GPU optimization of material point methods|GPU MPM Optimization]] — prior GPU MPM work by same group
