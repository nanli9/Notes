---
paper_id: "10.1145/1189762.1206075"
title: "Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies"
authors: ["Ingo Wald", "Solomon Boulos", "Peter Shirley"]
year: 2007
venue: "ACM Transactions on Graphics"
domain: "Vulkan_Ray_Tracing"
tags: [paper-notes, Vulkan-Ray-Tracing, BVH, deformable, SAH, foundational, BLAS-refit]
quality_score: 15.15
related_papers: ["10.1145/3687934", "10.1145/3658214", "10.1145/566654.566623"]
created: "2026-03-18"
status: "analyzed"
---

# Ray Tracing Deformable Scenes Using Dynamic Bounding Volume Hierarchies

**DOI:** [10.1145/1189762.1206075](https://doi.org/10.1145/1189762.1206075) · **Tag:** `10.1145/1189762.1206075`
**Authors:** Ingo Wald · Solomon Boulos · Peter Shirley
**Venue:** [[ACM Transactions on Graphics]] 2007 — **Foundational**

---

## Plain-English Summary

Shows that [[BVH|BVHs]] — previously dismissed as inferior to kd-trees — can match kd-tree performance on static scenes AND efficiently handle [[deformable body|deformable geometry]] by *refitting* the hierarchy each frame (updating bounding volumes without changing tree topology). The enabling insight is a novel packet-frustum hybrid traversal combining fast descent for hitting packets with fast exit for missing ones. This paper is the direct conceptual ancestor of [[Vulkan]] / DXR BLAS refit.

---

## Actual Novelty

At publication, kd-trees were the consensus best structure for ray tracing. Three contributions flipped this: (1) [[SAH]]-based [[BVH]] construction that matches kd-tree quality, (2) topology-fixed refitting for deforming geometry — O(n) per-frame update instead of full rebuild, (3) packet-frustum hybrid traversal. Contribution (2) is precisely what the Vulkan API calls "BLAS refit" today.

---

## Method

- Build [[BVH]] using Surface Area Heuristic ([[SAH]]) — same quality criterion as best kd-trees
- For [[deformable body|deformable geometry]]: fix tree topology, refit bounding volumes per frame (O(n))
- Packet traversal: trace groups of coherent rays together through the [[BVH]]
- Frustum traversal: early-exit for packets that miss a node's bounding volume entirely
- Hybrid: dynamically switch between packet and frustum strategies per node

---

## Evidence Quality

| Criterion | Status |
|---|---|
| Baselines named | ✅ SAH kd-trees (gold standard of the era) — direct named comparison |
| Quantitative metrics | ✅ frame rates, traversal step counts, static and deformable benchmarks |
| Runtime / memory reported | ✅ central to paper |
| Failure cases shown | ✅ explicitly: refitting degrades under large deformations (topology mismatch) |

---

## Likely Weaknesses

- Refit quality degrades significantly under large-amplitude deformations — periodic full rebuild needed
- Packet traversal assumes ray coherence — secondary/indirect rays (AO, GI) are incoherent and benefit less
- Implementation details now dated (2007); modern Vulkan/DXR provides hardware-accelerated BLAS refit

---

## Verdict

Required background reading for anyone working with [[Vulkan]] RT and deforming geometry. Read to understand *why* the BLAS refit/rebuild API tradeoff exists — not for implementation details.

---

## Related Papers

- [[20_Research/Papers/Vulkan Ray Tracing/3D Gaussian Ray Tracing Fast Tracing of Particle Scenes|3D Gaussian Ray Tracing]] — applies hardware RT to particle/Gaussian scenes; builds on this BVH foundation
- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics for Cloth and Shell Animation]] — deforming cloth geometry that needs BVH updates
- [[Robust Treatment of Collisions, Contact and Friction for Cloth Animation]] — collision detection with the same deforming geometry
