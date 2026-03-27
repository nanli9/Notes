---
paper_id: "10.1145/3550454.3555448"
title: "Pattern-Based Cloth Registration and Sparse-View Animation"
authors: ["Oshri Halimi", "Tuur Stuyck", "Donglai Xiang", "Timur Bagautdinov", "He Wen", "Ron Kimmel", "Takaaki Shiratori", "Chenglei Wu", "Yaser Sheikh", "Fabian Prada"]
year: 2022
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, cloth, registration, tracking, capture, data-driven]
quality_score: 14.65
related_papers: ["10.1145/3658214", "10.1145/566654.566623"]
created: "2026-03-19"
status: "analyzed"
---

# Pattern-Based Cloth Registration and Sparse-View Animation

**DOI/arXiv:** [10.1145/3550454.3555448](https://doi.org/10.1145/3550454.3555448) · **Tag:** `10.1145/3550454.3555448`

## Plain-English Summary

Capturing and tracking cloth deformation in the real world is hard because cloth lacks distinctive surface features. This paper prints a specially designed pattern onto garments, enabling precise per-point video tracking across multiple camera views. The tracked points are triangulated into registered mesh sequences with stable correspondence over time. They also propose "Garment Avatar" — a model driven from just two opposing cameras that reconstructs cloth geometry faithfully.

## Actual Novelty

The custom pattern design is the core contribution: it enables sub-centimeter tracking accuracy on deforming cloth using standard multi-view setups. Unlike markerless methods that struggle with cloth's self-similarity, the pattern gives unambiguous correspondence. The sparse-view driving model (2 cameras) is a practical bonus for interactive applications.

## Method

- Custom printed pattern on garment fabric, designed for robust video tracking
- Multi-view triangulation of tracked pattern points into 3D registered mesh
- Fine-grained geometric registration with temporally stable correspondence
- Garment Avatar model: dense tracking signal from 2 opposing views → reconstructed geometry
- Data release: 4 subjects, 15k frames, registered mesh sequences + pattern design

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to state-of-the-art markerless cloth capture methods |
| Metrics | Registration error, correspondence stability, driving fidelity |
| Runtime | Not prominently reported |
| Failure cases | Pattern must be physically printed; occluded pattern regions degrade |

## Likely Weaknesses

- Requires physically modifying the garment (printed pattern) — not applicable to arbitrary cloth
- Pattern visibility depends on lighting and camera resolution; dark/shiny fabrics may fail
- The 2-camera Garment Avatar needs the pattern — not a general markerless method
- Registration quality degrades under heavy self-occlusion (folding, layering)

## Verdict

Practical and well-executed capture pipeline. The pattern-based approach is refreshingly honest — instead of solving the impossible (markerless cloth tracking), it changes the problem. Valuable as ground-truth data generation for training data-driven [[cloth simulation]] models. The released dataset (15k frames) is a genuine contribution. Not applicable if you can't modify the garment.

## Related Papers

- [[20_Research/Papers/Simulation/Progressive Dynamics for Cloth and Shell Animation|Progressive Dynamics]] — simulation-side cloth; this paper provides capture-side data
