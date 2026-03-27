---
aliases: [skinning, linear blend skinning, LBS, dual quaternion skinning, DQS, pose-space deformation, PSD]
tags: [topic, Animation, deformable]
---

# Skinning

Deforming a mesh surface based on an underlying skeleton. The standard technique for character animation in games and film.

## Course Notes

### Linear Blend Skinning (LBS)
- Each bone $j$ defines a rigid transform: $X \to t_j + R_j \cdot X$, $j = 1, 2, \ldots, B$
- Skinned position: $x = \sum_{j=1}^{B} w^j (t_j + R_j \cdot X)$ where $w^j$ are bone weights
- Equivalent: $x = \sum_{j=1}^{B} w^j t_j + \left(\sum_{j=1}^{B} w^j R_j\right) X$
- Matrix form: $\begin{bmatrix} R_j & t_j \\ 0 & 1 \end{bmatrix} \begin{bmatrix} X \\ 1 \end{bmatrix}$

### LBS Artifacts
- **Candy wrapper:** twisting causes volume collapse (weighted average of rotations ≠ rotation)
- **Collapsing elbow:** bending causes mesh to shrink at the joint

### Solutions

#### 1. Helper Bones
- Extra bones placed at problem areas to correct deformation

#### 2. Dual Quaternion Skinning (DQS)
- Convert each bone transform $(t_i, R_i)$ to unit quaternion $q_i$, then to dual quaternion: $\hat{q}_i = (q_i, \frac{1}{2} t_i \cdot q_i)$
- Blend: $\hat{q} = \sum_{j=1}^{B} w_j(X) \cdot \hat{q}_j = \sum_{j=1}^{B} w_j(X) (q_j, \frac{1}{2} t_j \cdot q_j)$
- Dual quaternion $\hat{q} = (\hat{q}_0, \hat{q}_\varepsilon)$
- Normalize: $\text{Norm}(\hat{q}) = \left(\frac{\hat{q}_0}{\|\hat{q}_0\|}, \frac{\hat{q}_\varepsilon}{\|\hat{q}_0\|} - \frac{\hat{q}_\varepsilon \cdot \hat{q}_0}{\|\hat{q}_0\|^3} \hat{q}_0\right)$
- Extract: $\hat{q}_1' = \frac{1}{2} t \cdot \hat{q}_0'$, then $t = 2 \hat{q}_1' \cdot \hat{q}_0'^{-1}$
- Avoids candy-wrapper and collapsing-elbow artifacts

#### 3. Pose-Space Deformation (PSD)
- $\text{PSD}(P) = \text{skin}(P, X) + \partial(P)$ — skinning + correction function
- Correction $\partial: \mathbb{R}^\theta \Rightarrow \mathbb{R}^{3n}$ maps from pose space to displacement
- Training: given example poses $P_1, \ldots, P_m$ with target shapes $\chi_1, \ldots, \chi_m$
  - Displacements: $d_i = \chi_i - \text{skin}(P_i, X)$

#### Radial Basis Functions (RBF) for PSD
- $\partial(P) = \text{RBF}(P) = \sum_{i=1}^{m} \Phi(P - P_i) \cdot a_i$
- $\Phi: \mathbb{R}^\theta \to \mathbb{R}$, basis function
- Options: $\Phi(q) = \|q\|^\alpha$ or polyharmonic splines $\Phi(q) = \psi(\|q\|)$ where $\psi(z) = z^{2k} \log z$

## Papers
- [[20_Research/Papers/Simulation/Fast Simulation of Skeleton-Driven Deformable Body Characters|Fast Simulation of Skeleton-Driven Deformable Body Characters]]
