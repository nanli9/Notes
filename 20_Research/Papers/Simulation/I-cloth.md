---
paper_id: "10.1145/3272127.3275005"
title: "I-cloth"
authors: ["Min Tang", "Tongtong Wang", "Zhongyuan Liu", "Ruofeng Tong", "Dinesh Manocha"]
year: 2018
venue: "ACM Transactions on Graphics (SIGGRAPH Asia 2018)"
domain: "Simulation"
tags: [paper-notes, Simulation, cloth, GPU, collision, contact, spatial-hashing]
quality_score: 9.65
related_papers: ["10.1145/3450626.3459787", "10.1145/3528223.3530085", "10.1145/566654.566623"]
created: "2026-03-22"
status: "analyzed"
---

# I-cloth

**DOI/arXiv:** [10.1145/3272127.3275005](https://doi.org/10.1145/3272127.3275005) · **Tag:** `10.1145/3272127.3275005`

## Plain-English Summary
I-cloth is a GPU-based [[cloth simulation]] system that achieves interactive performance on complex garments (hundreds of thousands of vertices) by exploiting temporal and spatial coherence in collision handling. Instead of recomputing all collisions from scratch each solver iteration, it incrementally updates only the vertices that have moved, using [[spatial hashing]] and continuous collision detection (CCD). A non-linear impact zone solver on the GPU resolves penetrations, and the system combines this with implicit integration to allow large time steps.

## Actual Novelty
The key contribution is the **incremental continuous collision detection** scheme — tracking which vertices actually deformed between solver iterations and only recomputing collisions for those. This is paired with a GPU-parallel non-linear impact zone solver. The combination allows CCD-quality [[contact]] resolution at interactive rates, which was previously too expensive for GPU cloth pipelines.

## Method
1. **Implicit time integration** with large time steps for stable cloth dynamics
2. **Incremental CCD:** maintains a set of "dirty" vertices that moved beyond a threshold; only these are re-tested for collisions using [[spatial hashing]]
3. **GPU impact zone solver:** non-linear iterative solver that groups nearby colliding primitives into impact zones and resolves them in parallel on the GPU
4. **Coherence exploitation:** both spatial (hash grid reuse) and temporal (dirty vertex tracking) coherence reduce per-iteration collision cost

## Evidence Quality

| Aspect | Assessment |
|--------|-----------|
| Baselines | Compared against prior GPU cloth methods |
| Metrics | Frame rate (FPS), collision-free percentage, vertex count scaling |
| Runtime | 2–8 FPS for ~100k–300k vertices on commodity GPU (2018 era) |
| Failure cases | Not explicitly discussed; incremental scheme may miss collisions if coherence assumption breaks (e.g., sudden large deformations) |

## Likely Weaknesses
- **2018-era GPU performance:** 2–8 FPS is no longer competitive with modern GPU cloth solvers (e.g., [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU Cloth Wrinkles]] achieves higher resolution at better rates on newer hardware)
- **Coherence assumption:** incremental CCD relies on small per-iteration deformations; explosive or highly dynamic scenarios may degrade to full recomputation
- **No friction model detail:** the abstract mentions collision response but not frictional contact quality
- **Impact zone solver convergence:** non-linear solvers in impact zones may stall on deeply entangled configurations

## Verdict
Solid TOG paper that introduced a practical incremental collision detection pipeline for GPU [[cloth simulation]]. The 7–10x speedup over prior methods was significant at the time. The core idea — only recompute collisions for vertices that actually moved — remains relevant and has influenced later GPU collision pipelines. However, the absolute performance numbers are dated by 2025 standards.

## Related Papers
- [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU Cloth Wrinkles (2021)]] — later GPU cloth work achieving higher resolution
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid (2015)]] — complementary solver-side acceleration for cloth
- [[20_Research/Papers/Simulation/Robust Treatment of Collisions Contact and Friction for Cloth Animation|Robust Collisions (2002)]] — foundational cloth collision/contact work
