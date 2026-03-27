---
title: "GPU Simulation"
type: topic
tags: [topic, Simulation, GPU, parallel, real-time]
---

# GPU Simulation

Running physics simulation on GPU to exploit massive parallelism. Critical for interactive and real-time simulation.

**Key challenges:**
- Irregular memory access patterns (sparse matrix ops, contact)
- Synchronization between parallel threads (conflict-free writes)
- Solver convergence on GPU (iterative methods preferred)

**Common GPU-friendly methods:**
- [[PBD]] / XPBD — highly parallel, used in games
- Jacobi / Gauss-Seidel iterations — parallelizable with graph coloring
- Multilevel preconditioners — [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]]
- [[MPM]] — particle-grid transfers are GPU-amenable (Taichi, Warp)

**Key solvers:**
- [[PCG]] with GPU preconditioners — standard for [[FEM]] on GPU
- Conjugate Residual, GMRES — for asymmetric systems

## Papers
- [[GPU Multilevel Additive Schwarz Preconditioner for Cloth and Deformable Body Simulation]]
- [[3D Gaussian Ray Tracing Fast Tracing of Particle Scenes]] — GPU RT pipeline
- [[20_Research/Papers/Simulation/GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels|GPU-based Simulation of Cloth Wrinkles at Submillimeter Levels]]
- [[20_Research/Papers/Simulation/A Massively Parallel and Scalable Multi-GPU Material Point Method|A Massively Parallel and Scalable Multi-GPU Material Point Method]]
- [[20_Research/Papers/Vulkan Ray Tracing/Ray Tracing on Programmable Graphics Hardware|Ray Tracing on Programmable Graphics Hardware]]
