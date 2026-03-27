---
paper_id: "10.1145/566654.566579"
title: "Graphical Modeling and Animation of Ductile Fracture"
authors: ["James F. O'Brien", "Adam W. Bargteil", "Jessica K. Hodgins"]
year: 2002
venue: "ACM Transactions on Graphics"
domain: "Simulation"
tags: [paper-notes, Simulation, fracture, FEM, ductile, plasticity]
quality_score: 10.15
related_papers: ["10.1145/311535.311550", "10.1145/3306346.3322949"]
created: "2026-03-19"
status: "analyzed"
---

# Graphical Modeling and Animation of Ductile Fracture

**DOI:** [10.1145/566654.566579](https://doi.org/10.1145/566654.566579) · **Tag:** `10.1145/566654.566579`

## Plain-English Summary

Extends O'Brien & Hodgins' 1999 brittle [[fracture simulation]] to handle **ductile** materials — metals, plastics, and other materials that deform plastically before breaking. The key observation is that ductile fracture results from the interaction between plastic yielding and the fracture process: material first yields (permanent deformation), accumulates damage, and eventually fractures. The modification to the brittle framework is surprisingly simple and computationally inexpensive.

## Actual Novelty

Adds plastic yielding to the existing [[FEM]] fracture pipeline. When stress exceeds the yield threshold (but below fracture threshold), the material permanently deforms rather than fracturing immediately. Plastic strain accumulates over time, and fracture eventually occurs when the material can no longer accommodate further deformation. This simple extension dramatically expands the range of materials that can be simulated — from bent metal bars to crushed plastic toys.

## Method

1. Same [[FEM]] tetrahedral simulation as O'Brien & Hodgins 1999
2. **Yield criterion:** when stress exceeds yield strength $\sigma_Y$ (but below fracture strength $\sigma_{\text{crit}}$):
   - Compute plastic strain increment via return mapping
   - Permanently deform the rest shape of the element
3. **Fracture criterion:** unchanged — maximum principal stress > $\sigma_{\text{crit}}$
4. The interaction: plastic deformation redistributes stress, changes where and when fracture occurs
5. Material behavior controlled by: $\sigma_Y$ (yield), $\sigma_{\text{crit}}$ (fracture), hardening modulus

### Brittle vs. Ductile Spectrum
- $\sigma_Y \approx \sigma_{\text{crit}}$ → brittle (fractures before significant yielding)
- $\sigma_Y \ll \sigma_{\text{crit}}$ → ductile (large plastic deformation before fracture)
- Everything in between → mixed mode

## Evidence Quality

| Aspect | Assessment |
|---|---|
| Baselines | Compared to brittle-only method (O'Brien 1999) |
| Metrics | Visual plausibility — bending metal, crushing plastic |
| Runtime | Overhead from plasticity is minimal over base fracture sim |
| Failure cases | Not explicitly discussed |

## Likely Weaknesses

- Still relies on fragile remeshing for crack surfaces
- Plastic deformation model is relatively simple (no hardening curve, no Bauschinger effect)
- No temperature dependence (real ductile fracture is temperature-sensitive)
- Crack tip stress singularity not resolved — depends on mesh resolution

## Verdict

Elegant extension that gets maximum visual impact from minimal algorithmic complexity. The brittle→ductile generalization by adding a yield surface is textbook-correct and the implementation is straightforward. Together with the 1999 paper, this pair defines the FEM fracture paradigm in graphics. Adam Bargteil's first major contribution, launching his research career in fracture and destruction simulation.

## Related Papers

- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|Brittle Fracture (O'Brien & Hodgins 1999)]] — predecessor
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM]] — modern MPM alternative with damage mechanics
