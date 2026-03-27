---
title: "Fracture Simulation"
type: topic
tags: [topic, Simulation, fracture, deformable, destruction]
---

# Fracture Simulation

Simulation of crack initiation, propagation, and material failure in solid objects.

**Methods:**
- [[FEM]]-based stress-tensor fracture — crack initiates where stress exceeds a threshold, propagates along principal stress directions. Foundational: O'Brien & Hodgins (SIGGRAPH 1999)
- Peridynamics — non-local continuum mechanics; handles discontinuities naturally without special crack-tracking
- Spring-mass fracture — simpler, less accurate
- [[MPM]] — handles fracture through particle separation; natural for large deformation + fracture
- Voronoi / pre-fracture — precomputed fracture patterns for real-time (games)
- Clustered shape matching with fracture — real-time, Bargteil's group

**Industry use:**
- Digital Molecular Matter (Pixelux) — FEM fracture in 200+ films; derived from O'Brien's research
- Houdini Voronoi fracture — standard VFX pipeline tool
- APEX Destruction (UE4) — Voronoi for games

**Key lineage:**
- James O'Brien (PhD, Georgia Tech) → fracture via FEM stress tensors → Academy Award (2015)
- Adam Bargteil (PhD under O'Brien) → peridynamic fracture, clustered shape matching fracture

**Open problems:**
- Real-time FEM-quality fracture
- Fracture + fluid coupling (wet fracture)
- Neural fracture prediction
- Procedural fracture for interactive authoring

---

## Detailed Notes

### 1. Stress & Strain Review (Fracture Context)

Fracture is fundamentally about **when and where stress exceeds material strength**.

- **Cauchy stress tensor:** $\sigma \in \mathbb{R}^{3\times3}$, symmetric — describes internal forces at a point
  - Normal stress $\sigma_{ii}$ → tension/compression
  - Shear stress $\sigma_{ij}, i \neq j$ → sliding
- **Strain tensor:** $\varepsilon = \frac{1}{2}(\nabla u + \nabla u^T)$ (infinitesimal), or Green-Lagrange $\mathbf{E} = \frac{1}{2}(F^T F - I)$ for large deformation
- Constitutive law connects them: $\sigma = C : \varepsilon$ (linear elastic), or via energy density $\psi(F)$ for hyperelastic materials ([[FEM]])

### 2. Fracture Criteria — When Does It Break?

#### Maximum Principal Stress (Rankine Criterion)
- Eigendecompose $\sigma$: eigenvalues $\sigma_1 \geq \sigma_2 \geq \sigma_3$ (principal stresses), eigenvectors $\hat{n}_1, \hat{n}_2, \hat{n}_3$ (principal directions)
- **Fracture initiates when:** $\sigma_1 > \sigma_{\text{crit}}$ (tensile strength of material)
- Crack plane is **perpendicular** to $\hat{n}_1$ (the direction of maximum tension)
- This is what O'Brien & Hodgins 1999 use — simple, physically motivated

#### Von Mises Criterion (Ductile Materials)
- $\sigma_{\text{VM}} = \sqrt{\frac{1}{2}[(\sigma_1 - \sigma_2)^2 + (\sigma_2 - \sigma_3)^2 + (\sigma_3 - \sigma_1)^2]}$
- Fracture when $\sigma_{\text{VM}} > \sigma_Y$ (yield strength)
- Better for metals, plastics — materials that deform plastically before breaking

#### Griffith's Energy Criterion
- Crack propagates when the energy released by crack growth exceeds the energy cost of creating new surface
- Energy release rate: $G = -\frac{d\Pi}{dA}$ where $\Pi$ is potential energy, $A$ is crack area
- Fracture when $G \geq G_c$ (critical energy release rate, a material property)
- More physically grounded than stress criteria but harder to compute in graphics

#### Stress Intensity Factor (SIF)
- $K_I, K_{II}, K_{III}$ — mode I (opening), II (sliding), III (tearing)
- Fracture when $K_I \geq K_{IC}$ (fracture toughness)
- Related to Griffith: $G = K_I^2 / E$ (plane stress)

### 3. FEM-Based Fracture (O'Brien & Hodgins Approach)

The standard high-fidelity method in graphics.

#### Algorithm
1. Simulate deformable body with tetrahedral [[FEM]]
2. At each time step, compute Cauchy stress $\sigma$ per element
3. Eigendecompose $\sigma$ → principal stresses & directions
4. If $\sigma_1 > \sigma_{\text{crit}}$ in any element:
   - Crack plane passes through element centroid, normal = $\hat{n}_1$
   - **Split the mesh** along the crack plane
5. Remesh around the crack → new tetrahedra on each side
6. Continue simulation with updated mesh

#### Mesh Splitting
- The hard part: topological surgery on tetrahedral mesh
- Duplicate nodes along crack surface → two free surfaces
- Need to maintain mesh quality (avoid degenerate tetrahedra)
- O'Brien & Hodgins: split elements intersected by crack plane, re-tetrahedralize locally
- **Ductile fracture extension** (O'Brien, Bargteil, Hodgins 2002): allow plastic deformation before failure

#### Strengths & Weaknesses
- ✅ Physically accurate crack paths (stress-driven)
- ✅ Handles both brittle and ductile fracture
- ❌ Mesh splitting is complex and error-prone
- ❌ Remeshing can degrade element quality over time
- ❌ Expensive — not real-time

### 4. Voronoi Pre-Fracture (Game/Real-Time Method)

#### Algorithm
1. **Pre-processing:** fill object interior with Voronoi cells (random seed points)
2. Each cell becomes a separate rigid or deformable piece
3. At runtime, when impact force exceeds threshold → **break glue constraints** between cells
4. Pieces separate and simulate as independent rigid bodies

#### Implementation
- Voronoi diagram: partition space into cells closest to each seed point
- Cell boundaries = potential fracture surfaces
- Can control fracture pattern by seed distribution:
  - Uniform random → generic shatter
  - Clustered near impact → localized damage
  - Biased along material grain → directional fracture

#### Strengths & Weaknesses
- ✅ Fast — no mesh splitting at runtime, just constraint removal
- ✅ Deterministic, artist-controllable
- ✅ Standard in game engines (UE, Unity) and VFX (Houdini)
- ❌ Crack paths are predetermined (not physics-driven)
- ❌ Fracture surfaces look faceted (Voronoi artifact)
- ❌ Cannot model progressive cracking or crack branching

### 5. Peridynamics

Non-local continuum mechanics reformulation that naturally handles discontinuities.

#### Key Idea
- Classical continuum: forces via spatial derivatives of stress ($\nabla \cdot \sigma$) — undefined at cracks
- Peridynamics: forces via **integral** over a neighborhood (horizon $\delta$)
- Each material point $x$ interacts with all points $x'$ within distance $\delta$
- Force: $f(x) = \int_{H_x} \mathbf{f}(u(x') - u(x), x' - x) \, dV'$
- **Bond-based:** pairwise forces between points — like springs with finite range
- Bond breaks when stretch exceeds critical value → crack forms naturally

#### Fracture in Peridynamics
- No special crack tracking needed — bonds simply break
- Crack path emerges from which bonds fail
- Handles crack branching, merging, and multiple simultaneous cracks naturally
- Damage: $d(x) = 1 - \frac{\text{intact bonds}}{\text{original bonds}}$ ($d = 0$: undamaged, $d = 1$: fully broken)

#### In Graphics
- Levine et al. 2014 — peridynamic fracture for graphics
- Often discretized with particles → meshfree, no remeshing needed
- Can be coupled with [[SPH]] or [[MPM]] for multi-physics

### 6. MPM Fracture

[[MPM]] (Material Point Method) handles fracture through particle separation — when particles move apart beyond a threshold, the material is considered fractured.

- Particles carry full material state (stress, velocity, deformation gradient)
- Grid transfers handle force computation
- Fracture: damage field on particles, reduce stress when damaged
- Topology change is free — no mesh to split
- Natural for coupled scenarios: fracture + granular + fluid
- Wolper et al. 2019 (CD-MPM) — phase-field fracture with MPM
- Wolper et al. 2020 — anisotropic fracture with MPM

### 7. Spring-Mass Fracture

Simplest approach for real-time applications.

- Object represented as mass-spring network
- When spring force exceeds threshold → remove the spring
- Fracture = removing edges from the graph
- ✅ Trivial to implement
- ❌ Mesh-dependent crack paths (cracks follow edges)
- ❌ Not physically accurate — fracture pattern depends on mesh topology, not stress state
- Improved: add randomness to spring strengths for more natural patterns

### 8. Clustered Shape Matching Fracture

Real-time method combining shape matching deformation with fracture.

- Shape matching: each cluster of particles tries to match its rest shape via SVD
- Overlapping clusters → smooth deformation
- Fracture: when strain in a cluster exceeds threshold, split the cluster
- Sub-clusters continue simulating independently
- ✅ Real-time capable, stable
- ❌ Less accurate than FEM fracture

### 9. Practical Considerations

#### Material Properties Reference
| Material | $\sigma_{\text{crit}}$ (MPa) | Behavior |
|---|---|---|
| Glass | 30–50 | Brittle, clean fracture |
| Concrete | 2–5 (tensile) | Brittle, crumbling |
| Steel | 250–500 | Ductile, bends then breaks |
| Wood | 40–100 (along grain) | Anisotropic, splinters |
| Ice | 1–3 | Brittle, temperature-dependent |

#### Sound Generation
- Fracture events generate characteristic sounds
- Modal analysis of fragments → vibration frequencies → synthesized audio
- Impact sound ∝ fragment size and material stiffness

#### Debris & Secondary Effects
- Fracture produces: large fragments + small debris + dust particles
- Often simulated in layers: rigid body sim (large) + particle system (small) + volume rendering (dust)

## Papers
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Brittle Fracture|Graphical Modeling and Animation of Brittle Fracture]] — O'Brien & Hodgins, SIGGRAPH 1999. **Foundational.** FEM stress-tensor fracture, 524 citations, Academy Award lineage.
- [[20_Research/Papers/Simulation/Graphical Modeling and Animation of Ductile Fracture|Graphical Modeling and Animation of Ductile Fracture]] — O'Brien, Bargteil & Hodgins, TOG 2002. Extends brittle fracture with plastic yielding for metals/plastics.
- [[20_Research/Papers/Simulation/CD-MPM|CD-MPM: Continuum Damage Material Point Method]] — Wolper et al., SIGGRAPH 2019. Phase-field fracture + MPM. No remeshing. Modern standard for particle-based fracture.
- [[20_Research/Papers/Simulation/High-Resolution Brittle Fracture Simulation with Boundary Elements|High-Resolution Brittle Fracture with BEM]] — Hahn & Wojtan, TOG 2015. BEM-based, surface-only discretization, extremely detailed crack surfaces.
- [[20_Research/Papers/Simulation/Fast Approximations for Boundary Element Based Brittle Fracture Simulation|Fast BEM Brittle Fracture]] — Hahn & Wojtan, SIGGRAPH 2016. Linear-time crack propagation + rigid-body coupling.
- [[20_Research/Papers/Simulation/Simulating Fractures With Bonded Discrete Element Method|Simulating Fractures With Bonded Discrete Element Method]] — Lu et al., TVCG 2022. Bonded DEM for fracture — bonds break naturally, no remeshing, superior scale consistency over FEM. Explicit integration limits speed.
- Müller et al. 2013 — real-time dynamic fracture
- Bargteil et al. 2014 — peridynamic spring-mass fracture (SCA)
