---
aliases: [boids, flocking, flocking simulation, swarm]
tags: [topic, Simulation, Animation]
---

# Boids (Flocking Simulation)

Agent-based simulation of flocking behavior (birds, fish, crowds). Each agent (boid) follows simple local rules that produce emergent group behavior.

## Course Notes

### Steering Rules
Total acceleration: $a = a_{\text{alignment}} + a_{\text{separation}} + a_{\text{cohesion}} + a_{\text{obstacle}}$

All rules are distance-weighted — closer neighbors have stronger influence ($1/r_i^2$ weighting).

#### Separation
- Steer away from nearby boids to avoid crowding
- $a_{\text{separation}}^k \propto \sum_i \frac{1}{r_i^2} \hat{n}_i$, where $\|n_i\| > 0$
- $\hat{n}_i$ points from neighbor $i$ away from boid $k$

#### Alignment
- Steer toward average heading of neighbors
- $a_{\text{alignment}}^k \propto \left(\sum_i \frac{1}{r_i^2} v_i\right) \cdot \frac{1}{\sum_i \frac{1}{r_i^2}}$
- Weighted average velocity of neighbors (bold $i$ relative to $k$)

#### Cohesion
- Steer toward average position of neighbors
- $a_{\text{cohesion}}^k \propto \left(\sum_i \frac{1}{r_i^2} x_i\right) \cdot \frac{1}{\sum_i \frac{1}{r_i^2}}$
- $i$ relative to $k$

### Neighbor Finding — Spatial Hashing
- Divide space into grid cells
- Hash cell coordinates → hash table → lists of boid indices (e.g. A, B, C)
- Only check boids in the same and adjacent cells → $O(n)$ average instead of $O(n^2)$
