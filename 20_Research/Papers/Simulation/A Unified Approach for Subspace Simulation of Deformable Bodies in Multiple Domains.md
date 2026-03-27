---
paper_id: "10.1145/2816795.2818065"
title: "A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains"
authors: ["Xiaofeng Wu", "Rajaditya Mukherjee", "Huamin Wang"]
year: 2015
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, deformable, reduced-order, domain-decomposition, subspace, FEM]
quality_score: 12.40
related_papers: ["10.1145/2816795.2818089", "10.1145/1531326.1531359", "10.1145/3658184"]
created: "2026-03-22"
status: "analyzed"
---

# A Unified Approach for Subspace Simulation of Deformable Bodies in Multiple Domains

**DOI/arXiv:** [10.1145/2816795.2818065](https://doi.org/10.1145/2816795.2818065) · **Tag:** `10.1145/2816795.2818065`

## Plain-English Summary
Simulating large [[deformable body]] models in real time by splitting them into separate domains, each with its own reduced subspace. Domains are connected via "coupling elements" whose elastic properties match the rest of the body, avoiding manual stiffness tuning. The whole system — subspace deformations plus rigid motions — solves in a single linear system.

## Actual Novelty
- Coupling elements as a domain decomposition mechanism for [[reduced-order]] simulation — previous multi-domain subspace methods suffered from gap artifacts or locking at domain boundaries
- Single unified linear solve for both subspace deformations and rigid body motions across all domains
- Two cubature optimization schemes (uniform and non-uniform weights) for fast evaluation of reduced elastic forces from coupling elements

## Method
- Partition deformable body into disjoint domains; each domain's deformation is constrained to a low-dimensional subspace (modal basis)
- Coupling elements bridge adjacent domains — they share the same constitutive model as the rest of the body, so no artificial stiffness parameters
- At runtime: assemble reduced forces/Jacobians from both intra-domain and coupling elements, solve a single linear system for all domain DOFs
- Cubature approximation accelerates reduced force evaluation in coupling regions

## Evidence Quality

| Aspect | Assessment |
|--------|------------|
| Baselines | Compared against full-space [[FEM]] and previous multi-domain methods |
| Metrics | Visual quality, speedup factor, gap/locking artifacts |
| Runtime | Real-time for complex multi-domain scenes |
| Failure cases | Not explicitly discussed; cubature approximation quality depends on training |

## Likely Weaknesses
- Subspace basis is precomputed — topology changes (fracture, cutting) would invalidate it
- Coupling element cubature training adds to offline cost
- The approach assumes linear or mildly nonlinear deformations within each domain's subspace; extreme deformations may need basis updates
- No collision/[[contact]] handling discussed — would need external contact solver

## Verdict
Solid contribution to [[reduced-order]] [[deformable body]] simulation with a clean domain decomposition formulation. The coupling element idea avoids the parameter-tuning headaches of penalty-based domain coupling. Good fit for real-time applications with large, pre-segmentable models. The 2015 work predates newer neural subspace methods like [[20_Research/Papers/Simulation/Simplicits|Simplicits]] but the domain decomposition concept remains relevant.

## Related Papers
- [[20_Research/Papers/Simulation/Simplicits|Simplicits]] — mesh-free neural reduced-order simulation
- [[20_Research/Papers/Simulation/Deformable Object Animation Using Reduced Optimal Control|Deformable Object Animation Using Reduced Optimal Control]] — reduced-space optimization for deformable animation
- [[20_Research/Papers/Simulation/Expediting Precomputation for Reduced Deformable Simulation|Expediting Precomputation for Reduced Deformable Simulation]] — faster precomputation for the same class of methods
