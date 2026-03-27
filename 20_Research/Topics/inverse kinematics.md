---
aliases: [inverse kinematics, IK, forward kinematics, FK]
tags: [topic, Animation]
---

# Inverse Kinematics

Given a desired end-effector position, find the joint angles that achieve it. Inverse of forward kinematics.

## Course Notes

### Forward Kinematics (FK)
- Joint angles $\theta$ → end-effector position $(x, y)$
- $FK: \begin{bmatrix} x \\ y \end{bmatrix} = F(\theta)$

### Inverse Kinematics (IK)
- End-effector target $(x, y)$ → joint angles $\theta$
- $IK: (x, y) \mapsto \theta$
- Generally **underdetermined** (more DOF than constraints) or nonlinear

### Jacobian-Based IK
- Linearize: $\begin{bmatrix} x' \\ y' \end{bmatrix} = F(\theta + \Delta\theta) \approx F(\theta) + J \cdot \Delta\theta + O(\|\Delta\theta\|^2)$
- Jacobian: $J = \frac{\partial F}{\partial \theta}$
- Solve: $J \cdot \Delta\theta = \begin{bmatrix} x' \\ y' \end{bmatrix} - F(\theta) = \Delta x$
- Update: $\theta \to \theta + \Delta\theta$

### Pseudo-Inverse Method
- When $J$ is not square (more joints than end-effector DOF):
- $\theta = J^+ \cdot \Delta x$ where $J^+ = J^T (J J^T)^{-1}$
- $\Delta x = J J^+ \Delta x = (J J^T)(J J^T)^{-1} \Delta x = I \cdot \Delta x$ ✓
- Note: $J J^+ \neq I$ in general (only left-inverse)

### Tikhonov Regularization (Damped Least Squares)
- Problem: pseudo-inverse can be unstable near singularities
- Regularized objective: $\mathcal{E}(\Delta\theta) = \frac{1}{2} \|J \Delta\theta - \Delta x\|^2 + \frac{\lambda}{2} \|\Delta\theta\|^2$
- $\Delta\theta = \arg\min_{\Delta\theta} \mathcal{E}$
- $\frac{d\mathcal{E}}{d\Delta\theta} = 0 \Rightarrow (J^T J + \lambda I) \Delta\theta = J^T \Delta x$
- $\lambda$ controls trade-off: accuracy vs. stability
- Damping prevents joint velocities from exploding near singularities
