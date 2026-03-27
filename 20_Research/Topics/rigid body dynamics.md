---
aliases: [rigid body dynamics, rigid body simulation, rigid body]
tags: [topic, Simulation]
---

# Rigid Body Dynamics

Simulation of objects that do not deform — only translate and rotate. Foundation for game physics engines and multi-body simulation.

## Course Notes

### Position & Orientation
- Point on body: $P(t) = x(t) + R(t) \cdot P_0$ where $x(t)$ is center of mass, $R(t) \in \mathbb{R}^{3\times3}$ is rotation, $P_0$ is rest-pose offset (constant)
- Velocity: $\dot{P} = \dot{x} + \dot{R} \cdot P_0 = v_{\text{linear}} + v_{\text{rotation}}$

### Angular Velocity
- $v_{\text{rotation}} = \omega \times (R \cdot P_0) = \omega \times (P - x)$
- $\dot{R} \cdot P_0 = \omega \times (R \cdot P_0) = \tilde{\omega} R P_0$
- Therefore: $\dot{R} = \tilde{\omega} R$
- **Skew-symmetric matrix:** $\omega = [w_1, w_2, w_3]^T$, $\tilde{\omega} = \begin{bmatrix} 0 & -w_3 & w_2 \\ w_3 & 0 & -w_1 \\ -w_2 & w_1 & 0 \end{bmatrix}$
- Cross product as linear mapping: $y \in \mathbb{R}^3$, $y \mapsto \omega \times y = \tilde{\omega} \cdot y = A \cdot y$

### State Vector
$$X = \begin{bmatrix} x(t) \\ P(t) \\ R(t) \\ L(t) \end{bmatrix}, \quad \dot{X} = \begin{bmatrix} P/m \\ F \\ I(\omega)^T L(t) \cdot R \\ \tau \end{bmatrix}$$

### Linear Momentum
- $P = m \cdot v(t)$
- $\dot{P} = F(t)$ (Newton's second law)
- $\dot{x} = P/m$

### Center of Mass
- $x_{\text{cm}} = \frac{1}{m} \int r(t) \, dm$

### Angular Momentum
- $L(t) = I(t) \cdot \omega(t)$
- $\dot{L} = \tau$ (torque)
- Torque: $\tau = r \times F$
- Angular velocity from momentum: $\omega = I(t)^{-1} L(t)$

### Inertia Tensor
- $I(t) = \int \rho (\|r\|^2 I_{3\times3} - r r^T) \, dV \in \mathbb{R}^{3\times3}$
- "How mass is spread out" — $\rho$ is mass density
- **Key result:** $I(t) = R(t) \cdot I_0 \cdot R(t)^T$
  - $I_0$ is the inertia tensor in the undeformed (rest) frame — computed once
  - At runtime, just rotate: $I(t) = R \cdot I_0 \cdot R^T$

### Derivation of $I(t) = R \cdot I_0 \cdot R^T$
- $I(t) = \int \rho(\|r\|^2 I_{3\times3} - r \cdot r^T) \, dV$
- Substitute $r = R \cdot r'$ where $r'$ is rest-frame position
- $= \int \rho(\|R r'\|^2 I_{3\times3} - R r' r'^T R^T) \, dV$
- $= R \left[\int \rho(\|r'\|^2 I_{3\times3} - r' r'^T) \, dV\right] R^T$
- $= R \cdot I_{\text{undeform}} \cdot R^T$
